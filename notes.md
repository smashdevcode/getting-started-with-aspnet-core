
# Notes


## Presentation

Give overview of ASP.NET Core
  What is it?
  Why was it created?

Mac OS X
  Setup your env
  Create a new console app
  Turn it into a basic web app
  Turn it into a basic MVC app
  Show debugging in VS Code

Windows
  Start with VS Community installation
  What else needs to be installed?
  Show the experience of creating a new project

Features to show
  Built-in DI
  Combined MVC and Web API surface
  Tag helpers



## Questions

Can controller still be just plain old classes???

When you referece a method (i.e. `app.UseStaticFiles()`) that requires a new dependency to be added...
  Does VS Code give you a shortcut to add that dependency like VS proper does?

Is there still an IIS native loader?
  I thought that Kestrel was the only app server?

What are the specific parts of the project.json file?

What is the difference between the project.json when using VS Code and VS proper?

What are the files that are generated into the bin folder?

What are the files that VS Code adds to your project?
  How do these compare to the files that VS proper requires?

Do ASP.NET Core apps run on a single thread?
  Take a look at how apps are hosted from IIS

Are action methods async by default?
  Or do you need to deliberately make your action methods async?





## TODO




Determine what is the minimum code required to create a valid web project

Determine what is the minimum code required to create a valid MVC web project

Review the source code for the MVC methods that are used to configure services and middleware for MVC





Create new Windows 10 VM (using Virtual Box or Fusion?)
  Install VS Community and Update 3

Check out VS extension that grabs a dependency's source code
  How difficult would it be to create a command line version of this?

Review the source code for the methods that are used to create a web app





Migrating from ASP.NET 5 RC1 to ASP.NET Core 1.0
  https://docs.asp.net/en/latest/migration/rc1-to-rtm.html#update-target-framework-monikers-tfms

Review docs.asp.net
  Check out the Migration docs - https://docs.asp.net/en/latest/migration/mvc.html

Work up simple Web API and EF Core sample
  Get familiar with the new migration commands
  https://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html

ASP.NET Core forms
  https://docs.asp.net/en/latest/mvc/views/working-with-forms.html

Dig into Identity for ASP.NET Core

Work up a full-stack web app example
  Web API
  EF Core
  Postgres
  React or Angular 2?
  TypeScript
  Webpack
  RxJS

Work examples for...
  Configuration
  Logging
  Custom middleware
  Tag helpers

Convert Treehouse projects to ASP.NET Core

Practice debugging web apps in VS Code

Setup Linux VM with .NET Core

Try deploying to Azure






## Building a Web App with ASP.NET Core - Shawn Wildermuth

### What is ASP.NET Core?

Why reinvent ASP.NET?
  The web stack is old
  Weighed down by code in System.Web that wasn't used anymore
  Dependency on machine level upgrades was problematic
  Performance was as good as it could get

ASP.NET Core is a complete rethinking of the platform
  Your knowledge about how to build specific parts will still be useful
  But the underlying platform is different

What is ASP.NET Core?
  Complete rewrite of the platform
  Cross platform and open source
  MVC and Web API are combined, Web Forms is gone
  Everything is a dependency (pay for play)
  Low memory footprint
  Multiple deployment support

ASP.NET Core Overview
  Runs on the full .NET Framework, .NET Core, or Mono

.NET Core is a subset of the full .NET Framework

Completely Composed
  Everything above the .NET layer is a NuGet package
  Frameworks are the bootstrap and CLR
  MVC, StaticFiles, Logging, Configuration, Identity, etc. are all just packages
  Everything is optional

Embraces Open Web Development
  Use existing web development tooling
    NPM, Bower, Gulp, Grunt, NuGet

Installing ASP.NET Core
  dot.net site
  SDK vs VS official MSI installer
  The VS installer installs two new project templates
    ASP.NET Core Web Application (.NET Core and .NET Framework)

Hello World ASP.NET Core
  `dotnet` command line tool
  `dotnet --help` to see a list of the available built-in commands
  The list of available commands is extensible
    You can add your own commands
  `project.json` file
    `emitEntryPoint` tells the compiler to emit an entry point that points to the `void Main()` method in `Program.cs`
    `dependencies` are NuGet files
      `Microsoft.NETCore.App` is the base library for .NET Core apps
        The runtime is defined as a dependency of the app, not just globally available
    `frameworks` defines what platform versions this project can work with
      Options include
        `netcoreapp1.0`
        `net451`
        `net452`
        `net46`
      `imports` defines what flavors of the platform this project can work with
        Uses the PCL naming convention
        Options include
          `dnxcore50`
  `dotnet restore` restores all of your project's dependencies
  `dotnet run` will build and run our app
  `dotnet build` will build the app without running it

VS Code
  Download for free
  Available cross platform
  You can open projects using `code .` in the current working directory
  Support for C# needs to be installed
  VS Code will ask you if it's okay to add items to your project to support your project
  No notion of how to create new projects
  You can use yeoman to create projects
    Yeoman requires Node.js and NPM to both be installed
    `npm install yo -g` to install yeoman
    `npm install generator-aspnet -g` to install the yeoman ASP.NET templates
    `yo aspnet` to generate a project

Visual Studio
  To check if .NET Core has been installed...
    Run VS and create a new project
    You should see a couple of ASP.NET Core Web Application project templates available
  Extensions
    Web Essentials
    Web Compiler
    Add New File
    Open Command Line
    TypeScript
  Create a new "Empty" project (to keep things simple at first)
  `Program.cs`
    `void Main` method is the starting point for your web app
    WebHostBuilder
      `UseKestrel` is the web server to use for your web app
      `UseContentRoot` specifies where the content lives
      `UseIISIntegration` adds support for IIS (mostly for specialized headers for Windows Auth)
      `UseStartup` specifies the class to instantiate when the server starts up
  `Startup.cs`
    `ConfigureServices`
    `Configure`
      Sets up ALL of the code (i.e. middleware) to be ran every time a request is processed
      Middleware is ran in the order that it's defined in
      No other "hidden" or "secret" handlers or modules
  Future tooling changes
    `project.json` and `.xproj` will be replaced with a `.csproj` file

### HTML and CSS Basics

Serving Files in ASP.NET Core
  All static files must be located in the `wwwroot` folder
  The web server must be configured to serve static files

```
app.UseDefaultFiles(); // this must come before the static files middleware
app.UseStaticFiles();
```

The order of middleware is very important
  Review the source code for the `UseDefaultFiles` method

Nothing is implicit anymore
  Everything in the ASP.NET pipeline is explicit

### JavaScript




### MVC 6

First Controller/View
  Need to add MVC to the project
  Package is `Microsoft.AspNetCore.Mvc`
  Controllers inherit from `Controller`

Enabling MVC 6
  Add the MVC middleware by calling `app.UseMvc()`
  Configure the MVC services by calling `services.AddMvc()`
  Pass in a lambda into the `UseMvc` method call to configure your default route

```
app.UseMvc(config => 
{
  config.MapRoute(
    name: "Default",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "App", action = "Index" }
  );
});
```

Creating a Layout
  Use _ViewStart.cshtml file to globally set a `Layout` view property
  Be sure to use `~` at the beginning of all static file paths

Adding More Views
  Server errors by default just return `500` Server Error HTTP status codes
  You can configure developer errors by calling `app.UseDeveloperExceptionPage()`
    This middleware is in its own package `Microsoft.AspNetCore.Diagnostics`
  You can change your configuration by the current environment

```
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
  if (env.IsDevelopment())
  {
    app.UseDeveloperExceptionPage();
  }
}
```

  This is just looking at the `ASPNETCORE_ENVIRONMENT` environment variable
    You can create your own environment variables

Using Tag Helpers
  Need to add a dependency for `Microsoft.AspNetCore.Mvc.TagHelpers`
  The _ViewImports.cshtml page can be used to add global imports for each view
    `@addTagHelper "*, Microsoft.AspNetCore.Mvc.TagHelpers"`
  To create links...
    `<a asp-controller="App" asp-action="About">About</a>`

Implementing a Contact Page
  The `asp-for` tag helper can be used to associate form elements to model properties

```
@model MyModel

<form>
  <label asp-for="Prop"></label>
  <input asp-for="Prop" />
  <button>Submit</button>
</form>
```

Using Validation
















## Smallest Web App

1) Run the `dotnet new` command.

2) Add `"Microsoft.AspNetCore.Server.Kestrel": "1.0.0" as a dependency.

3) Open `Program.cs` and add the following namespaces.

```
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;
```

And add the following code in the `Main` method.

```
new WebHostBuilder()
    .UseKestrel()
    .Configure(a => a.Run(c => c.Response.WriteAsync("Hello world!")))
    .Build()
    .Run();
```

4) Then `dotnet restore` and `dotnet run`.

## dotnet CLI

GitHub Repo
[https://github.com/dotnet/cli](https://github.com/dotnet/cli)


## .NET Standard

https://docs.microsoft.com/en-us/dotnet/articles/standard/index












## Resources

Where's DNVM? Safely running multiple versions of the .NET Core SDK and Tooling with global.json
http://www.hanselman.com/blog/WheresDNVMSafelyRunningMultipleVersionsOfTheNETCoreSDKAndToolingWithGlobaljson.aspx

Exploring dotnet new with .NET Core
http://www.hanselman.com/blog/ExploringDotnetNewWithNETCore.aspx
