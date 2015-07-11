---
layout: post
title: "Caliburn Micro: Focus TextBox when View gets visible"
description: ""
category: CaliburnMicro, WPF
tags: Caliburn Micro, WPF
---
Currently I'm using the WPF framework [Caliburn Micro](http://caliburnmicro.com/) for my UI. With *Conductors*  you can manage the lifecycle of views. If an item gets activated/visible I'd like to focus the TextBox to enhance the user experience. Therefor I've created a new *Behaviour*:

    public class FocusWhenVisibleBehavior : Behavior<FrameworkElement>
    {
        protected override void OnAttached()
        {
            AssociatedObject.Loaded += Loaded;
            AssociatedObject.IsVisibleChanged += VisibleChanged;
        }
 
        protected override void OnDetaching()
        {
            AssociatedObject.Loaded -= Loaded;
            AssociatedObject.IsVisibleChanged -= VisibleChanged;
        }
 
        private void Loaded(object sender, RoutedEventArgs e)
        {
            TryFocus();
        }
 
        private void VisibleChanged(object sender, DependencyPropertyChangedEventArgs e)
        {
            TryFocus();
        }
 
        private void TryFocus()
        {
            if (AssociatedObject.IsLoaded && AssociatedObject.IsVisible)
            {
                AssociatedObject.Focus();
            }
        }
    }

Now you can use it in the XAML file:

    <TextBox x:Name="Input">
          <i:Interaction.Behaviors>
              <Behaviour:FocusWhenVisibleBehavior/>
          </i:Interaction.Behaviors>
    </TextBox>

i is the namespace *System.Windows.Interactivity* which is included in the [Blend SDK](https://www.microsoft.com/en-US/download/details.aspx?id=10801). So you have to install the SDK on your build server, reference a [third party NuGet package](https://www.nuget.org/packages/Expression.Blend.Sdk/) or wait if Caliburn Micro will include the package ([Issue #145](https://github.com/Caliburn-Micro/Caliburn.Micro/issues/145)). 
