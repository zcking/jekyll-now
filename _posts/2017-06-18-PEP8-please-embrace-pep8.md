---
layout: post
title: "PEP8: Please Embrace PEP8"
categories: programming
comments: true
---

{% include alert.html content="<b>Heads Up!</b> This post is a port of an older post from my old blog" level="info" %}

I know this is my first post here, but before I pull my soapbox out I'd like to address those of you who may be wondering (from the title) what is *PEP8*? First off, please realize this post is about the Python programming language. Okay? Cool.

PEP stands for Python Enhancement Proposals. Each proposal has a number identifying it; a full index of the PEPs is available [here](https://www.python.org/dev/peps/). Now then, PEP8 in particular is referred to as the *official* Python style guide and can be read [here](https://www.python.org/dev/peps/pep-0008/).

Although PEP8 is not mandatory (not required in order for code to be functional), it is the official convention for Python code. Now I know, I know...there are a lot of conventions out there for myriad languages and let's face it, hardly any of them are obliged by the majority of the world's developers. That's where I'd like to step in and say please let Python's PEP8 be your one exception, if anything.

Python was originally created with readability as one of its primary cornerstones. PEP8 further enhances this goal and I think we should all (as Pythonic programmers) adopt it's style guide as much as possible.

There are various advantages to using the PEP8 style guide. For example, it states you should always provide a file docstring at the beginning of your Python file/script; I always include one in mine indicating the name of the file, the author (moi), and a brief description of what the file/script does.

Another point in the PEP8 style guide is that you are expected to always provide docstrings for functions and classes. This is especially useful when it comes to sharing your code and having other programmers use the interactive `help(...)` function on the code. This built-in method of  Python utilizes these docstrings to describe what the function or class does; furthermore, these docstrings can contain example usages of the function or class, which can even be executed via the [docstring module](https://docs.python.org/3/library/doctest.html).

There are plenty of other benefits to adapting the PEP8 style guide to your Python code, which I won't go into here. However, if you're unfamiliar with it and would like to learn more, please check out the link I provided above. Finally, I would like to make a note of how I use the PEP8 style guide. I use Visual Studio Code as my primary text editor for writing Python currently, and you can easily install a PEP8 Python linter/extension for this editor which uses colored lines to indicate warnings and "errors" as defined by the PEP8 style guide. So, if you're interested in using PEP8 but are want an easy way of transitioning to it and ensuring your adherence to it, [this Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python)  helps tremendously.
