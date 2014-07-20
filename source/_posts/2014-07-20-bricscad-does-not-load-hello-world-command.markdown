---
layout: post
title: "BricsCAD does not load \"Hello world\" command"
date: 2014-07-20 15:55:19 +0200
comments: true
categories: BricsCAD
---

## Problem
BricsCAD does not show the command after loading the assembly with NETLOAD.

{% codeblock lang:csharp %}
[CommandMethod("Go")]
public void Go()
{
    MessageBox.Show("Hello world");
}
{% endcodeblock %}

<!--more-->

## Solution
There is no API call in this file. 
At least one command has to do something with the BricsCAD API. E.g write something to the console, open a transaction or just update the screen.
{% codeblock lang:csharp %}
[CommandMethod("Go")]
public void Go()
{
    MessageBox.Show("Hello world");
    Bricscad.ApplicationServices.Application.UpdateScreen();
}
{% endcodeblock %} 