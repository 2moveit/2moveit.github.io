---
layout: post
title: "Use one code base for AutoCAD and BricCAD"
date: 2014-07-21 21:50:47 +0200
comments: true
categories: BricsCAD
---
[BricsCAD](https://www.bricsys.com) form BricsSys supports the same API as [AutoCAD](http://www.autodesk.com/products/autocad/overview) from Autodesk.
Same API but not the same interface. So how is it possible to use just one code base and support both cad systems? 

First create a configuration for AutoCAD `Debug` and BricsCAD `DebugBricsCAD`. This will make it easy to start either AutoCAD or BricsCAD from Visual Studio as you can define which program should start in the project properties:
{% img center /images/posts/2014-07-21Settings.png %}
To switch the interfaces in you source code you have to change the using statements depending on the cad system you want to use.
You can manage this with preprocessor statements:
{% codeblock lang:csharp %}
#if BRICSCAD
using Teigha.DatabaseServices;
using Teigha.Runtime;
using Teigha.Geometry;
 
using Bricscad.ApplicationServices;
using Bricscad.Runtime;
using Bricscad.EditorInput;
using Application = Bricscad.ApplicationServices.Application;
using Platform = Bricscad;
using PlatformDB = Teigha;
#else
using Autodesk.AutoCAD.DatabaseServices;
using Autodesk.AutoCAD.Runtime;
using Autodesk.AutoCAD.Geometry;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.EditorInput;
using Autodesk.AutoCAD.Windows;
using Application = Autodesk.AutoCAD.ApplicationServices.Core.Application;
using Platform = Autodesk.AutoCAD;
using PlatformDB = Autodesk.AutoCAD;
#endif
{% endcodeblock %}
To switch the configuration you need to define the symbol BRICSCAD.
Go to `project properties` -> `Build` -> `General` -> `Conditional compilation symbols` and add `BRICSCAD` there. Make sure that the configuration `DebugBricsCAD` is selected. If you like you can add `AUTOCAD` as conditional compilation symbol to the `Debug` configuration. As we do not check this value it is not required here but if you later want to support [nanoCAD](http://www.nanocad.com) from neosoft you might need it for the `#elseif`.
{% img center /images/posts/2014-07-21Constant.png %}
By doing this the conditional compilation symbol is added in the project file (.proj) to the tag `<DefineConstants>` and it will be used for the preprocessor statements. 
{% codeblock lang:xml %}
<PropertyGroup Condition="'$(Configuration)|$(Platform)' == 'DebugBricsCAD|AnyCPU'">
    <DebugSymbols>true</DebugSymbols>
    <OutputPath>bin\DebugBricsCAD\</OutputPath>
    <DefineConstants>TRACE;DEBUG;BRICSCAD</DefineConstants>
    <DebugType>full</DebugType>
    <PlatformTarget>AnyCPU</PlatformTarget>
    <ErrorReport>prompt</ErrorReport>
    <CodeAnalysisRuleSet>MinimumRecommendedRules.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>
{% endcodeblock %}
 Now you can start your implementation with one code base and start the cad systems as you like by just switching the configuration `Debug` and `DebugBricsCAD`.
  




