---
layout: post
title: "Use a condition to copy just specific assemblies"
date: 2014-07-22 20:13:17 +0200
comments: true
categories: MsBuild
---
In my [previous post](./blog/2014/07/21/use-one-code-base-for-autocad-and-bricscad/) I showed how to use conditional compilation symbols to switch between using statements for AutoCAD and BricsCAD. For both system the interface assemblies are referenced and `copy local` is set to `false`.
In a similar case where `copy local` is set to `true` you will notice that the referenced assemblies of both configuration are copied into the output directory. As we want to switch the whole system this isn't what we want.
To avoid this you can use a condition so that just the assemblies of one configuration will be in you output directory.

Unload the project and open it in the editor window. Then add the condition to check the `DefineConstants` and to decide if the assembly will be included or not:
{% codeblock lang:xml %}
 <Reference Condition="$(DefineConstants.Contains(BRICSCAD)) Include="BrxMgd">
    <HintPath>lib\BrxMgd.dll</HintPath>
    <Private>False</Private>
</Reference>
{% endcodeblock %}