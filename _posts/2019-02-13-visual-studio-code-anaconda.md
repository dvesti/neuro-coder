---
layout: single
title:  "How to run anaconda environment in Visual Studio Code terminal"
date:   2018-09-20 16:16:01 +0100
categories: programming
tags: python anaconda visual-studio-code tips
---

I love [Visual Studio Code](https://code.visualstudio.com/). I tried Atom, I tried Sublime, I use Notepadd++ for text editing, but for javascript coding, markdown, blogging and occasionally python coding I use VSC. Atom always seemed sluggish, despite all my effort and even PC upgrade, and sublime was nice and fast, but it just misses bunch of programming features. VSC has somehting that I just fell in love since the first time I used it, and that was the integrated terminal. 

If you have Linux or a Mac, you are solid. If you run Windows, you might be slightly underwhelmed by the command prompt capabilities, until you install [Chocolatey](https://chocolatey.org/). I already sang praise about Chocolatey [here]({{ site.baseurl }}{% post_url 2018-09-23-how-to-run-jekyll-on-windows %}), and it is a must for a Windows based programmer in my opinion. So now you have VSC running and you can prototype your code nad run it from the same window, amazing. BUT ...

The main issue I came across was to run python code in [Anaconda](https://anaconda.org). I use Anaconda as it was recommended, therefore installing packages in separate environments for different purposes (see list of some of my envs [here](https://github.com/hejtmy/python-envs)). I install almost nothing in the *root* environment, which is the default one that command prompt starts with when you open it. Another issue is that for some reason, I couldn't change the VSC anaconda environment using the anaconda's `activate *env*` command and it was still defaulting to the *root*. So when I am building markdown documentation with [mkdocs](https://www.mkdocs.org/) and I need to load my *base* environment, I couldn't run it in the VSC prompt. Luckily, there was a simple solution.

Another great feature of the VSC is customizability in its settings file. The entire settings is in *.json*, which is genius, because you can easily share and copy settings between users or projects no hassle. No longer we are bound by describing to what menu you have to go to click on which button (love you `JetBrains` and I know you have your *.ini* files, but changing settings is often a chore). In VSC, you just create a *.vscode* folder with *settings.json* inside and you change what you wanna change. Difficult for a lay person, amazing for those who know what they are doing :)

```
project-folder
|-.vscode
|---settings.json
```

So if you want to run your python commands in the VSC terminal using a particular environment, you can just point it to that env with adding this line to the *settings.json* file.

```json
"python.pythonPath": "path-to-your-environment/python.exe" 
```

If you are unsure what the path is, you can always call `conda info --envs` and get it. This is output of mine,
```shell
ai                       C:\ProgramData\Anaconda3\envs\ai
base                     C:\ProgramData\Anaconda3\envs\base
root                  *  C:\ProgramData\Anaconda3
```

So in the end, if I want to run my project in the *ai* environment, I'd just change my settings.json to:

```json
//settings.json is platform agnostic in folder separators, so you can use "/" even on windows
{
	"python.pythonPath": "C:/ProgramData/Anaconda3/envs/ai/python.exe"
}
```