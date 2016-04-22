---
layout: post
title:  "Awesome Cake with Xamarin Part 1"
date:   2016-04-22 07:25:07 +0200
published: true
categories: xamarin cake
---

This is planned to be a series of posts, describing some ways to be more productive when developing with Xamarin and outling some aspects of doing automatic testing and deployments easily.

I will first go trough some basics on why you would want to automate your project:

1. **Less firefighting** - You will instantly detect when someone checks in some breaking changes in the project.
2. **Better refactoring** - Unless you are doing TDD/BDD (you really should!), the more complex your application becomes, the less confident you will be to do refactoring. Without testing you won't know if something breaks when you change your code.
3. **Save time** - Why do the boring work of building, preparing and uploading an app to App Store / Google Play? Let Cake do it for you.

Zhe Buildtool
---------
First and foremost the tool of choice here in this guide is [Cake](http://cakebuild.net).

*Cake (C# Make) is a cross platform build automation system with a C# DSL to do things like compiling code, copy files/folders, running unit tests, compress files and build NuGet packages.*

There are also lots of alternatives like Rake, psake, FAKE out there that you can choose from.

### Installation

The easiest ways to use Cake in your project is to use their bootstrapper.

**Windows**

	Invoke-WebRequest http://cakebuild.net/bootstrapper/windows -OutFile build.ps1

**Linux**

	curl -Lsfo build.sh http://cakebuild.net/bootstrapper/linux

**OS X**

	curl -Lsfo build.sh http://cakebuild.net/bootstrapper/osx

This will download everything need to start running Cake buildscripts.

### Cake script

The bootstrapper need a `build.cake` file to be able to run, so create one in the same directory as the bootstrapper.

{% highlight csharp %}

var target = Argument("target", "Default");

Task("Default")
  .Does(() =>
{
  Information("Hello Ninja!");
});

RunTarget(target);
{% endhighlight %}
