---
layout: post
title: "Change DebugType in multiple C# projects.rb"
date: 2014-08-13 21:15:27 +0200
comments: true
categories: ruby, script, c#
---

To change settings of multiple C# projects is quite easy with [Ruby](https://www.ruby-lang.org) and the [Nokogiri gem](https://rubygems.org/gems/nokogiri). The following script will change the DebugType of all Release configurations in multiple csproj files.

{% gist e0d7c30a04e955794dbe Change_debugtype_in_csproj.rb %}

At first the script gets all csproj files which are actually xml files. Each file is loaded into a @doc variable by using Nokogiri. Then the document is queried with XPath for the PropertyGroup tags. Each tag is validated if it matches the specified configuration. If it matches a query for the DebugType tag is executed. If it isn't found a new DebugType node with the specified DebugType value is created. Otherwise the available tag is changed. Finally the modified XML is written into the original project file.