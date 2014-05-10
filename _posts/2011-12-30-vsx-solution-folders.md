---
layout: post
title: VSX Solution Folder
tags: [C#, VSX]
share: true
---

Recently I've been working on few extensions for Visual Studio 2010. In a series of post I'll share different snippets and techniques that I've learned.

In this post, I'll show you how to create, and how to find an existing Solution folder.

First we need a reference to a DTE2 object ([http://bit.ly/szlrOt](http://bit.ly/szlrOt)) and then to a Solution2 object.

{% highlight csharp %}
DTE2 dte2 = ...; // Get a reference to DTE2 object.

Solution2 solution = (Solution2)dte2.Solution;
{% endhighlight %}

Then to add the folder, just invoke the AddSolutionFolder method and pass the name of the folder:

{% highlight csharp %}
Project projectItem = solution.AddSolutionFolder("MyNewSolutionFolder");

SolutionFolder solutionFolder = (SolutionFolder)(projectItem.Object);
{% endhighlight %}

There is trick, though. If a folder with the same name already exists an exception will be thrown. Here is a way to find an existing Solution folder:

{% highlight csharp %}
SolutionFolder foundSolutionFolder = null;
string folderName = "MyNewSolutionFolder";

foreach (Project project in solution.Projects)
{
	if (project.Kind == ProjectKinds.vsProjectKindSolutionFolder)
	{
		if (string.Compare(project.Name, folderName, StringComparison.CurrentCultureIgnoreCase) == 0)
		{
			foundSolutionFolder = (SolutionFolder)project.Object;
			break;
		}
	}
}
{% endhighlight %}

I hope that you will find this information useful.

'till next time.
