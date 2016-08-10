---
layout: post
title:	 "Awesome Cake with Xamarin Part 1"
date:   2016-04-22 07:25:07 +0200
published: false
categories: xamarin cake
---

*THIS IS WORK IN PROGRESS*

This is planned to be a series of posts, describing some ways to be more productive when developing with Xamarin and outling some aspects of doing automatic testing and deployments easily.

I will first go trough some basics on why you would want to automate your project:

1. **Less firefighting** - You will instantly detect when someone checks in some breaking changes in the project.
2. **Better refactoring** - Unless you are doing TDD/BDD (you really should!), the more complex your application becomes, the less confident you will be to do refactoring. Without testing you won't know if something breaks when you change your code.
3. **Save time** - Why do the boring work of building, preparing and uploading an app to App Store / Google Play? Let Cake do it for you.

### Some general recommendations

For the sake of this guide, I will make some assumptions about the project structure in your project. But in general you should try to follow this recommendations when building your projects to facilitate extensibility and good structure.

~~~
root directory/
	..
	src/ (main project code)
	tests/ (all test projects)
	tools/ (tools needed for testing and building, i.e cake)
	build.sh (the bootstrap file for cake)
	build.cake (the Cake file)
	..
~~~


Zhe Buildtool
---------
First and foremost the tool of choice here in this guide is [Cake](http://cakebuild.net).

*Cake (C# Make) is a cross platform build automation system with a C# DSL to do things like compiling code, copy files/folders, running unit tests, compress files and build NuGet packages.*

There are also lots of alternatives like Rake, psake, FAKE out there that you can choose from.

### 1. Installation

*The instructions here is from their official [GitHub Page](https://github.com/cake-build/cake) so please use it as a reference as it may contain newer instructions*.

The easiest way to use Cake in your project is to use their bootstrapper. In your project directory, run this in your terminal of choice:

#### Windows PowerShell

~~~powershell
Invoke-WebRequest http://cakebuild.net/bootstrapper/windows -OutFile build.ps1
~~~

#### Linux

~~~console
curl -Lsfo build.sh http://cakebuild.net/bootstrapper/linux
~~~ 

#### OS X
~~~console
curl -Lsfo build.sh http://cakebuild.net/bootstrapper/osx
~~~

It should download everything need to start running Cake buildscripts. Before you run the bootstrapper you need to set up a cake script.

### 2. Cake script

The bootstrapper need a `build.cake` file to be able to run, so create one in the same directory as the bootstrapper.

~~~csharp
var target = Argument("target", "Default");

Task("Default")
  .Does(() =>
{
  Information("Hello Ninja!");
});

RunTarget(target);
~~~

### 3. Run it!

##### Windows

~~~powershell
# Execute the bootstrapper script.
./build.ps1
~~~

##### Linux / OS X

~~~console
# Set your script to be executable by changing permissions
chmod +x build.sh

# Execute the bootstrapper script.
./build.sh
~~~

### Result
You should now see your cake script running and this output:

	Hello Ninja!

Building your project with Cake
-------------------------------
The most important thing to be able to test things in your project is to build it.

### Setup restore of dependencies with NuGet

We will create a new Task in Cake that will handle the restoration of packages. The alias used is [NugetRestore](http://cakebuild.net/api/cake.common.tools.nuget/bdfa6572/a0e10885).

~~~csharp
Task("Nuget-Package-Restore")
.Does(() =>
{
	// Path to Xamarin Solution
	var solution = GetFiles("./src/Awesome.Xamarin.Project/*.sln");
	NuGetRestore(appSolutions);  
});
~~~
