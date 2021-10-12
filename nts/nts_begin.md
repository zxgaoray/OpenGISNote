# **NTS**

# &star; Overview


# &star; Table of Content

&emsp;  
# DOTNET environment
## environment variables

## command line
### common
```shell
> dotnet --version
```

## package manager
### NuGet

&emsp;  
# vscode create project
## console
```shell
> dotnet new console --name SomeConsoleProjectName
```

## solution
```shell
> dotnet new sl -O SomeSolutionName
> dotnet new classlib -O SomeSolutionName.SomeProjectName
> dotnet sln add SomeSolutionName.SomeProjectName/SomeSolutionName.SomeProjectName.csproj
```
or:
> (1) dotnet new classlib -O SomeSolutionName.SomeProjectName  
> (2) dotnet new console -O SomeSolutionName.SomeProjectName
> (3) dotnet new xunit -O SomeSolutionName.SomeProjectName
