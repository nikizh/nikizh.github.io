---
layout: post
title: Find Hwnd by PID
tags: [C#]
share: true
---

The code below can be used to find a handle to a window using a process ID:

{% highlight csharp %}
using System.Runtime.InteropServices;

internal static class NativeMethods
{
    [DllImport("user32.dll", SetLastError = false)]
    public static extern IntPtr GetDesktopWindow();
    
    [DllImport("user32.dll", CharSet = CharSet.Unicode)]
    public static extern IntPtr FindWindowEx(IntPtr parentHandle, IntPtr childAfter, string lclassName, string windowTitle);
    
    [DllImport("user32.dll", SetLastError = true)]
    public static extern uint GetWindowThreadProcessId(IntPtr hWnd, out uint lpdwProcessId);
    
    [DllImport("user32.dll")]
    [return: MarshalAs(UnmanagedType.Bool)]
    public static extern bool IsWindowVisible(IntPtr hWnd);
    
    [DllImport("user32.dll", ExactSpelling = true, CharSet = CharSet.Auto)]
    public static extern IntPtr GetParent(IntPtr hWnd);
}

internal static class HwndFinder
{
    internal static IntPtr GetProcessWindow(int processId)
    {
        IntPtr prevWindow = IntPtr.Zero;

        while (true)
        {
            IntPtr desktopWindow = NativeMethods.GetDesktopWindow();

            if (desktopWindow == IntPtr.Zero)
                break;

            IntPtr nextWindow = NativeMethods.FindWindowEx(desktopWindow, prevWindow, null, null);

            if (nextWindow == IntPtr.Zero)
                break;

            uint procId = 0;
            NativeMethods.GetWindowThreadProcessId(nextWindow, out procId);

            if (procId == processId)
            {
                bool isWindowVisible = NativeMethods.IsWindowVisible(nextWindow);
                bool hasParent = NativeMethods.GetParent(nextWindow) == IntPtr.Zero;

                if (isWindowVisible && hasParent)
                {
                    return nextWindow;
                }
            }

            prevWindow = nextWindow;
        }

        return IntPtr.Zero;
    }
}
{% endhighlight %}