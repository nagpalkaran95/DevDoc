---
layout: post
title:  "Building a custom Dotnet CLI tool"
date:   2022-09-03 00:00:00 +0530
categories: csharp dotnet
background: '/img/dotnet/banner.jpg'
---

{: style="text-align: justify; font-size:25px"}
> If you have used any `.Net` framework before then you might be familiar with the `dotnet` command. We have been using quite a lot of .NET Core command line interface (CLI) tools, such as `dotnet new`, `dotnet build`, `dotnet add package`, `dotnet watch run`, and so on. Have you ever thought about building a similar CLI tool of you own?

<p></p><p></p>

### Problem Statement

{: style="text-align: justify;"}
You might come across a use case when none of the inbuilt commands provided by `dotnet` is suffice. Similarly I once wanted to create a custom command to perform some set of operations.

<p></p><p></p>

### Prerequisites

* Working knowledge of .Net

<p></p><p></p>

### Solution

**Build a custom dotnet tool**

{: style="text-align: justify;"}
In the .NET Core world, these tools are called Global Tools (link). A .NET Core Global Tool is a special NuGet package that contains a Console application, which can be invoked in a terminal via predefined commands in the Global Tool.

Although by convention it should be called a custom command but Microsoft calls it a `tool` so we'll go ahead with that nomenclature. When we say creating a custom dotnet tool, it just means a custom command/tool just like `dotnet run`. Here `run` is the tool.

{: style="text-align: justify;"}
**Note:** We can name our tool anything.

**Steps**

1. Create a Console project and add the code

    {: style="text-align: justify;"}
    This step involves whatever we usually do to create a Console app. For example, add a command line parser, add some unit tests, and so on.

2. Set up the global tool

    {: style="text-align: justify;"}
    We need to update the ***.csproj** file of the Console project. The following code snippet shows an example csproj file for a global tool.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <PackAsTool>true</PackAsTool>
        <ToolCommandName>helloworld-run</ToolCommandName>
        <PackageId>helloworld-run</PackageId>
        <PackageOutputPath>.nuget</PackageOutputPath>
      </PropertyGroup>

      <ItemGroup>
        <PackageReference Include="System.CommandLine.Experimental" Version="0.3.0-alpha.19405.1" />
      </ItemGroup>

    </Project>
    ```

    {: style="text-align: justify;"}
    The property `PackAsTool` tells the .NET build system to pack the project as a Global Tool.

    {: style="text-align: justify;"}
    The property `ToolCommandName` determines the command name that will be used to invoke the tool.

    {: style="text-align: justify;"}
    The property `PackageId` determines the file name of the output NuGet package.
    
    {: style="text-align: justify;"}
    The property `PackageOutputPath` is the release directory of the NuGet package. By default, the *.nupkg file will be saved to an output directory under the bin folder. We are using .nuget folder here, so that we can locate the output NuGet package easier.

    {: style="text-align: justify;"}
    **Note:** The default values for both `PackageId` and `ToolCommandName` are the project name.

3. Pack your application

    ```sh
    > dotnet pack

    ### giving custom package version
    # dotnet pack -p:PackageVersion=1.0.0
    ```

    {: style="text-align: justify;"}
    The above command will create a .nupkg file which we can publish it to the nuget.org or other package registries, so that the package is available within a shared network.

4. Install your tool

    ```sh
    ### For installing the tool locally for a project

    # cd into the project-folder where you want to install and use the custom tool
    > cd <project-folder>

    # create a tool manifest file which is local to the project
    > dotnet new tool-manifest

    # check the contents of the manifest file
    > less .config/dotnet-tools.json

    # install the tool
    > dotnet tool install --add-source /<path>/helloworld-run/.nuget helloworld-run #confirm that directory is having your .nupkg file

    # confirm your tool is installed
    > dotnet tool list
    ```

5. Using your tool

    ```sh
    dotnet tool run helloworld-run

    #or

    dotnet helloworld-run
    ```

6. Updating your tool

    {: style="text-align: justify;"}
    In case you update the code in your tool then you need to pack the application again using different packageVersion and run the following the command

    ```sh
    > dotnet tool update helloworld-run
    ```

7. Uninstalling your tool

    {: style="text-align: justify;"}
    If you want to uninstall the tool completely then run the following command
    ```sh
    > dotnet tool uninstall helloworld-run

    # confirm your tool is installed
    > dotnet tool list
    ```

<p></p><p></p>

### References
- <https://docs.microsoft.com/en-us/dotnet/core/tools/global-tools-how-to-create>
