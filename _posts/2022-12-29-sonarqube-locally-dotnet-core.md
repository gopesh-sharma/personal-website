---
layout: post
title: 'Get Started with SonarQube Locally using .Net Core'
categories: blog
tags: ['.Net Core', 'SonarQube', 'Testing']
excerpt: 'SonarQube is a leading Code Quality Assurance tool which collects and analyzes your code and it provides the report, the post is all about getting started with it locally and using .Net Core'
date: December 29, 2022
author: self
---

SonarQube is a leading Code Quality Assurance tool which collects and analyzes your code and it provides the report. It enables continuous quality measurement over time and combines tools for dynamic and static analysis. SonarQube checks and evaluates everything, from minor styling choices to design flaws. This gives users access to a comprehensive history of the code that can be searched. This allows them to see where the code is going wrong and figure out if it's styling issues, code defeats, code duplication, a lack of test coverage, or too much code.



### PreRequisites

To get started with SnarQube locally, you have to get some prerequisites software and they are:

1. Install JVM/JDK 11, if not already installed
2. Use Any Database, if nothing is installed then you can Install PostgreSQL.
3. Download the SonarQube [Community Edition](https://www.sonarqube.org/downloads/).
4. Download SonarQube  [Scanner](https://github.com/SonarSource/sonar-scanner-msbuild/releases/download/5.2.1.31210/sonar-scanner-msbuild-5.2.1.31210-netcoreapp2.0.zip).

UnZip SonarQube and SonarCube Scanner in a directory.

### Configuring Database

To use SonarQube locally we have to configure the database which SonarQube can use to show the results. I have setup PostgreSQL in my locally and configured the database and user as shown below:

1. Create DB - **CREATE DATABASE sonarqubedb;**
2. Create User - **CREATE USER sonarqubeuser WITH PASSWORD 'some-password';**
3. Grant Privileges - **GRANT ALL PRIVILEGES ON DATABASE sonarqubedb TO sonarqubeuser;**

Using the above, you have created the database **sonarqubedb** and user **sonarqubeuser** and given full access to **sonarqubeuser** to access **sonarqubedb** database.

### SonarQube Config

Now it's time to update the configuration. Go to the directory where you have unzipped the SonarQube Community edition and open properties from the path “**..\conf\sonar.properties.**”

Change the username and password as well as JDBC Postgres URL.

```
sonar.jdbc.username=sonarqubeuser
sonar.jdbc.password=some-password
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqubedb
```

After configuration, now SonarQube will use PostgresSQL to save reports or logs locally.

### Start SonarQube

Go to the directory where you have unzipped the SonarQube Community edition and run the StartSonar batch file from the path **“..\bin\windows-x86–64\StartSonar.bat”**. Make sure you are running it in admin mode.

Once the SonarQube server is up, a client application will be available on the browser at the following URL http://localhost:9000/projects

If you are facing any issues, then check the logs from the path **"..\logs\sonar.log"**.

### Working on SonarQube

Now it's time for activities where you will be creating a new project for analysis. 

#### Add a New Project

In this, we will be using the Manual option as of now, but you can use any option you want.

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/CreateProject.PNG' | absolute_url }}" alt="Create Project">
  <figcaption>Create Project</figcaption>
</figure> 

Then we will add the project details as shown below

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/CreateProject1.PNG' | absolute_url }}" alt="Create Project">
  <figcaption>Create Project</figcaption>
</figure> 

And then set Locally as how we want to analyze the repository as shown below

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/CreateProject2.PNG' | absolute_url }}" alt="Create Project">
  <figcaption>Create Project</figcaption>
</figure> 

**Note: The UI can change from version to version but the concept will be the same**

Then we will get the token for our repository to uniquely identify our project.

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/token.PNG' | absolute_url }}" alt="Create Token">
  <figcaption>Generating Token</figcaption>
</figure>

And then we will set up whether we will be using .Net Core or .Net Framework and after doing that we will get what needs to be done to analyze the repository. 

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/run.PNG' | absolute_url }}" alt="Running SonarQube Locally">
  <figcaption>Running SonarQube Locally</figcaption>
</figure>

As a prerequisite, analysis requires a sonar scanner tool installed globally using the following command:

```
dotnet tool install --global dotnet-sonarscanner
```

#### Execute the Scanner

Running a SonarQube analysis is straightforward. You just need to execute the following commands at the root of your solution.

```
dotnet sonarscanner begin /k:"SonarQubeTesting" /d:sonar.host.url="http://localhost:9000"  /d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88"
```

```
dotnet build
```

```
dotnet sonarscanner end /d:sonar.login="sqp_94c3910294432957166eb22f3596a4208d1bfc88"
```

**Note: Do note copy from above as your token will be different.**

### Sample Report 

After running we will get the initial report which is something like below:

<figure style="width: 600px" class="align-center">
  <img src="{{ '/images/blog/report.PNG' | absolute_url }}" alt="Sample report">
  <figcaption>SonarQube Sample Report</figcaption>
</figure>

Hope you are able to run the SonarQube locally on .Net Core, if you are facing any issues do let me know.
