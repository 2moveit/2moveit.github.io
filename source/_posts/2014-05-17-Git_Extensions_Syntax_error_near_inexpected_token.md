---
layout: post
title: "Git Extensions: Syntax error near unexpected token"
description: ""
category: Errors
tags: [Git, Error]
---

## Problem
If you try to push a repository you may get the message:
{% blockquote %}
"... syntax error near unexpected token '("
{% endblockquote %}

<!--more-->
	
## Solution
Change the "\\\" to just "\" in the file **C:\Users\< UserName >\\.gitconfig**

### Wrong:
    helper = !\\\"C:/Program Files (x86)/GitExtensions/GitCredentialWinStore/git-credential-winstore.exe\\\"

### Correct:
    helper = !\"C:/Program Files (x86)/GitExtensions/GitCredentialWinStore/git-credential-winstore.exe\"
