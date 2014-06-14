---
author: Chris Nicola
date: '2009-11-18 08:42:00'
layout: post
slug: automating-deployment-with-teamcity-and-psake
status: publish
title: Automating Deployment with TeamCity and psake
comments: true
wordpress_id: '49'
categories:
- .NET
- Software Development
- continuous deployment
- powershell
- psake
- TeamCity
---

![space_sake][1]

Recently, my projects have gotten to the point where simply building, testing and publishing in Visual Studio will no longer cut it.  There are many reasons to use an independent build server, the main one being the advantages of continuous integration.  However, out of the box a build server like TeamCity does little more than checkout, build and test your application and report and track the results.  What we really want is to integrate that into a deployment process.

This is where build automation tools can be handy.  There are many, many options here: [nant][3] is a popular one for XML lovers, so is [rake][4]**,** a ruby based option.  [MSBuild][5] can also be used, which is essentially what VS is using behind the scenes.  However both rake and custom msbuild files require to much angle bracket evilness.  Rake seems like it could be decent but I'm not working with ruby and I don't want to force everyone I work with to setup ruby either.  Fortunately, James Kovacs has created a powershell based build tool which finally offers a good non-XML option to those working in an MS world.

<!--more-->

You can get psake [here][6] and the download includes some usage examples, though they don't really show you how to use it to build visual studio projects.  Ayende has a relatively sophisticated example for his own projects [here][7] as well. 

I am going to split this into a couple of parts.  In the this part I just want to introduce psake and show to to create simple build tasks for compiling, testing and packaging.  In the next 1-2 parts I am going to I want to walk through how to use psake with TeamCity to automate deployment for both ASP.NET and .NET projects. 

I will start by mentioning that I am using psake 2.01 and powershell 2.0.  If you are using XP you will need to install SP3 in order to use powershell 2.0, you can get PS2.0 from [here][8].

## Part 1: Using psake to perform standard build tasks

Psake defines functions called "tasks" to help order the build process.  I should note there are a number of interesting things you can do with tasks that are new to psake 2.01.  For example, you can specify pre and post conditions and pre and post actions for a task for example. 

For the moment we will keep it simple and just use the -depends switch.  In this scenario a task is simply something you can command your build script to execute and each task can depend on other tasks to be completed before it in a hierarchical manner.  For example, **compile** may depend on **clean** and **test** may depend on **compile**.

Here is a basic psake script for building a simple ASP.NET web application:

```
properties { 
  $solution_name = "MyWebsite"
  $website_folder =   "MyWebsite"
  $base_directory = resolve-path .
  $tools_directory = "$base_directory\tools"
  $build_directory = "$base_directory\build"
  $publish_directory = "\\MyWebServer\website"
  $publish_user = "deployment"
  $publish_pass = "password"
  $local_test_runner = "C:\Program Files\NUnit 2.5.1\bin\net-2.0\nunit-console.exe" 
  $unittest_project = "$build_directory\UnitTests.dll"
} 

task Init -depends Clean { 
  new-item $build_directory -itemType directory | Out-Null
}

task Clean { 
  remove-item -force -recurse $build_directory -ErrorAction SilentlyContinue | Out-Null
}

task Package {
  try {
    &$tools_directory\7z.exe a -tzip "$build_directory/$solution_name.zip" "$build_directory/_PublishedWebsites/*"
  } catch {
    &echo "Error packaging artifacts"
    Exit 1;
  }
}

task Compile -depends Init { 
  try {
    &msbuild /verbosity:minimal /p:"Configuration=Release;Platform=Any CPU;OutDir=$build_directory\\" $solution_file
  } catch {
    &echo "Error compiling"
    Exit 1;
  }
}

task Test -depends Compile {
  try { &$local_test_runner $unittest_project }
  catch {
    &echo "Error starting test runner"
    Exit 1;
  }
}
```

The psake script is composed of two main sections.  The first is properties, where you set some global variables to be used throughout your script.  These can be used to make it easy to adapt your script to different projects, tools, etc.  The second are the tasks.  In this example I've created Init: creates a build folder, Clean: removes anything from a previous build, Compile: uses msbuild to build from the solution file, Test: runs the nunit test runner against my test project output and finally package, which uses 7zip to compress my build output.

To run this build script I will need to do the following in powershell, or in another powershell script:
    
```
Import-Module '.\psake.psm1' Invoke-psake '.\psakebuild.ps1' -framework 3.5 Remove-Module psake
```

If that seems a bit verbose, keep in mind you can place the psake.psm1 module in one of the powershell modul module [folders][9] which will make sure powershell always imports it.  For me, I just add that to a script called runbuild.ps1 and I'm good.

In the Invoke-psake command we pass the script above.  By specifying -framework 3.5 psake adds _Windows/MICROSOFT.NET/Framework/v3.5/_ to the path.  You can specify any framework version you need 2.0, 3.0, etc., it is an optional parameter.  You can also specify a task to execute, if not it will execute the **default** task.  The general syntax looks like this:
    
```
Invoke-psake <psake script file> <Task Name> -framework <version>
```

You may have noticed the try.catch blocks as well.  These are not necessary, but they provide a way for me to ensure errors are communicated to TeamCity.  TeamCity responds to non-zero exit codes by reporting a failed build which is what I want to see.

   [1]: /images/space_sake_thumb.jpg (space_sake)
   [2]: /images/space_sake.jpg
   [3]: http://nant.sourceforge.net/
   [4]: http://rake.rubyforge.org/
   [5]: http://msdn.microsoft.com/en-us/library/0k6kkbsd.aspx
   [6]: http://code.google.com/p/psake/
   [7]: http://ayende.com/blog/4156/on-psake
   [8]: http://support.microsoft.com/kb/968929
   [9]: http://tfl09.blogspot.com/2009/01/modules-in-powershell-v2.html

