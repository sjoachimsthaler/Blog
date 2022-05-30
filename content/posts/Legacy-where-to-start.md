+++
title = "Legacy App. Where should I even start..."
date = "2022-05-30T22:11:36+02:00"
draft = false
author = "Stefan Joachimsthaler"
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["legacy", "refactor", "devops", "CI/CD"]
description = ""
showFullContent = false
readingTime = false
+++

# How do you tackle the refactoring of a legacy app?

A big legacy app can be really frightening at first. But the good thing is, as we are working with software, there are some steps that can be done to ease the pain.
This is not special to any language or framework but can be used in every project you have to tackle.

## Step 1: Source Control
This one should not be necessary, but some apps are more legacy than others and can still be true. If you want to tackle a modernization of an app that is not under source control then breathe, take one step back and make sure you put in under source control.
This way you always have the possibility to go back to your previous state.

The preferred source control should be git. As most developers are familiar with it. But if it is running under another version control system or you or your team are more familiar with other tools then go for it.

## Step 2: Automate the build process
The next step I would do is to automate the build process.
Especially on older software there are often many steps necessary to build an application. If this is the case, then the developer feedback loop is not good. You want to have a fast failure if you break something in your modernization efforts.

If you have for example a Visual Studio solution then I would try to build it with ```dotnet build``` or ```msbuild```. If this works, great. If not, try to make it working. Restore Nuget packages, add configuration, do what you have to do. You can also use tools like [Cakebuild](www.cakebuild.org) or [Nuke](https://nuke.build/). The important part is to make it reproduceable. So you could put all your steps in a Powershell script or a .bat-file.

If you work on a Java project you should be able to build your project with ```javac``` or you could start to create a maven or ant script to build your project.

## Step 3: Put your automated build in a CI pipeline
The last two steps are a great base. Now is the time to put it together.

If you are working in a team (which is usually the case), then you need a way to build (and package) the software independent from a developer machine. This means you need a way to build your software independently from your computer with the press of a button. 
If you use Git as your version control system you have an army of possibilities with probably the most popular to host your repository on [Github](https://github.com). 

When your repo is hosted on Github you can make use of Github Actions to trigger a build when you push code to the repository. This ensures that  if you for example miss to add a file that is necessary to build the solution you will be informed in a timely manner and not when somebody else tries to release the software and can't because he does not have all the necessary files needed to do so.

## Final thoughts
Those three steps mark only the start of modernizing a software. But for me this is the bare minimum to even start to fix it. Ideally the next step would be to write some tests to ensure some modules can be safely refactored to improve the structure of a project. But this is a topic on its own and sometimes cannot even be achieved without too much hassle.