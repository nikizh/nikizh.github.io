---
layout: post
title: Finding the Pointer Position in WinRT
tags: [C#, WinRT]
share: true
---

It’s easy to find the position of the pointer if you can use the event arguments of one of the pointer events. However, if you are developing C#/Xaml application and you find yourself in situation where you need to get the position without using the events you can use the `PointerPosition` property exposed by `CoreWindow`.

There is one more little detail and we can dive into the code - if you have a setup with more than one monitor CoreWindow.PointerPosition will give you the absolute position of the cursor, not the position relative to the window of your WinRT app. To get the position inside the window we can use `CoreWindow.Bounds`, and more specifically `X` and `Y` of the bounds in order to get the offset of the window.

{% highlight csharp %}
using System;
using Windows.Foundation;
using Windows.Graphics.Display;
using Windows.UI.Xaml;

namespace PointerPositionSample
{
    public static class PointerUtils
    {
        public static Point GetPointerPosition()
        {
            Window currentWindow = Window.Current;

            Point point;

            try
            {
                point = currentWindow.CoreWindow.PointerPosition;
            }
            catch (UnauthorizedAccessException)
            {
                return new Point(double.NegativeInfinity, double.NegativeInfinity);
            }

            Rect bounds = currentWindow.Bounds;

            return new Point(point.X - bounds.X, point.Y - bounds.Y);
        }
    }
}
{% endhighlight %}

You might be wondering about this try/catch. For some (undocumented) reason accessing `PointerPosition` might throw `UnauthorizedAccessException` when the computer is locked.

So, that’s all you need to do in order to transform the `PointerPosition` for the need of your C#/Xaml app. I hope you find this useful.