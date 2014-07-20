---
layout: post
title: "Prevent command prompt's auto close"
description: ""
category: Batch
tags: [batch, snippet]
---

As I want to start a specific task of a rake script with a double-click in windows I had to created a batch file to start the task:
{% codeblock lang:powershell %}
rake -f rakefile.rb task_name
{% endcodeblock %}
Either if the task fails or not the command window will close. 
 
To solve this you have to add `pause` and `call` the rake script. As I'm just interested in the output in case of an error I check the errorlevel for it.
{% codeblock lang:powershell %}
call rake -f rakefile.rb unit_tests
if %ERRORLEVEL% == 1 (
	pause
)
{% endcodeblock %}