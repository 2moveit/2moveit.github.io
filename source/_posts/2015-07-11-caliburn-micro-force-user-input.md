---
layout: post
title: "Caliburn Micro: Force user input"
description: ""
category: CaliburnMicro, WPF
tags: Caliburn Micro, WPF
---
I want to force the user to enter some text into a `TextBox` before he can do anything else. As described in my [previous post](/2015/07/caliburn-micro-focus-textbox-when-view-gets-visible.htm) the `TextBox` will get the focus when it becomes visible. Now I have to prevent that it loses the focus.
To do that I bind `PreviewLostKeyboardFocus` and check if the TextBox is empty. If it is I just have to set the `Handled` property of the `KeyboardFocusChangedEventArgs` to `true`.

XAML binding:

    <TextBox x:Name="Input" cal:Message.Attach="[Event PreviewLostKeyboardFocus] = [Action OnPreviewLostFocus($source, $eventArgs)]"/>

View Model:

    public void OnPreviewLostFocus(System.Windows.Controls.TextBox textBox, KeyboardFocusChangedEventArgs args)
    {
         if (textBox.Text == "")
        {
            MessageBox.Show("Enter some text to continue.");
            args.Handled = true;
        }
    }
