---
layout: post
title: 'Generate Code Coverage using .Net Core or .Net Framework with SonarQube'
categories: blog
tags: ['.Net Core', 'SonarQube', 'Testing']
excerpt: 'SonarQube is a leading Code Quality Assurance tool which collects and analyzes your code and it provides the report, the post is all about showing code coverage with it locally and using .Net'
date: January 11, 2023
author: self
---

In my [previous](https://www.gopeshsharma.dev/blog/sonarqube-locally-dotnet-core/) post, I talked about getting started with SonarQube locally to analyze the code quality. In this post, we will discuss generating the code coverage report and show it SonarQube dashboard. So that we can see the overview of our code quality and see if there are styling issues, code defeats, code duplication, a lack of test coverage, or too much code.

In my [previous](https://www.gopeshsharma.dev/blog/sonarqube-locally-dotnet-core/) post we have seen how to execute the scanner where we will execute the following commands at the root of the solution,

```
dotnet sonarscanner begin /k:"SonarQubeTesting" /d:sonar.host.url="http://localhost:9000"  /d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88"
```

```
dotnet build
```

```
dotnet sonarscanner end /d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88"
```

## Generating Code Coverage using Coverlet

To generate the code coverage using the Coverlet we will be using the coverlet console which is the simplest way to set up and run the code coverage

### Installing Coverlet Console

```
dotnet tool install --global coverlet.console
```

### Usage

We can invoke the `coverlet` tool by specifying the path of the assembly that contains the unit tests. We have to use the `target` (Path to the test runner application) which is `dotnet` and the `targetargs` (Arguments to be passed to the test runner). We have used the format as opencover and the output file as XML. The following example shows how to use the coverlet for the test application.

```
coverlet "../Application.Tests.dll" 
	--target "dotnet" 
	--targetargs "test ../Application.Tests.csproj --no-build" 
	-f=opencover 
	-o="coverage.xml"
```

### Pushing the code coverage to SonarQube

You have to change the SonarQube analysis commands which you need to execute at the root of your solution.

```
SonarScanner.MSBuild.exe begin 
/k:"SonarQubeTesting" 
/d:sonar.host.url="http://localhost:9000"  
/d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88" 
/d:sonar.cs.opencover.reportsPaths=coverage.xml 
```

```
dotnet build
```

```
coverlet "../Application.Tests.dll" 
	--target "dotnet" 
	--targetargs "test ../Application.Tests.csproj --no-build" 
	-f=opencover 
	-o="coverage.xml"
```

```
SonarScanner.MSBuild.exe end /d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88"  
```

**Note: Do not copy from above as your token will be different.**

### Report

After running we will get the report with the code coverage which is something like the below:

![Report](https://gopeshsharma.dev/images/blog/report2.PNG)

Hope you are able to run the SonarQube locally with Code Coverage on .Net Core/.Net Framework, if you are facing any issues do let me know.
