
# Notes






## dotnetConf

http://www.dotnetconf.net/
https://channel9.msdn.com/Events/dotnetConf/2016


### .NET Conf Day 1 Keynote: Scott Hunter

https://channel9.msdn.com/Events/dotnetConf/2016/NET-Conf-Day-1-Keynote-Scott-Hunter

.NET Standard Library
  .NET Core is the first version of the Standard Library
  The Standard Library will grow over time

More APIs to come
  App Domains
  Full Reflection
  Binary Serialization

New additions to the Standard Library will be just additive
  Won't require you to rewrite or port your apps

F# is coming to .NET Core

Projects unification model
  Everything is CLI first
  Then the tools wrap those commands
  New project system
    `project.json` was built in mind for ASP.NET Core
    When embracing all of the other app models it was going to be a lot of work to make it work for everyone
  So... `project.json` is going away in favor of returning to MSBuild
  But wait... all of the cool stuff is not going away
    It might even be possible to still use a `project.json` file in limited scenarios
  All of the tooling--VS, VS Code, and Xamarin--will use the same project system

.NET Core RTM 7/27
  .NET Core
  ASP.NET Core
  EF Core
  Tooling Preview 2 (RTM with VS "15")

The RTM version of the tooling won't require you to port your apps
  VS will convert the project to use the new `.csproj` project system

Future changes
  SignalR will likely be the first
  Web Pages will also be coming (Razor views without controllers)

### ASP.NET Core – deep dive on building a real website with today’s bits

https://channel9.msdn.com/Events/dotnetConf/2016/ASPNET-Core--deep-dive-on-building-a-real-website-with-todays-bits

Agenda
  Logging and Diagnostics
  Configuration
  Publishing

Code is available at https://github.com/glennc/RealWorldAspNetCore

Focusing on middleware

`Startup.cs`
  Builds a pipeline for handling HTTP traffic
  Enables all of the capabilities that your application knows how to do

`UseStaticFiles`
  This piece of middleware is asking "is this request for a static file?"
  If that's a "yes", then it returns the file, and returns back through all of the previous middleware

If no middleware handles the request... a `404` is returned

Example of inline middleware
  ```
  app.Use((context, next) => 
  {
    context.Response.StatusCode = 404;
    return Task.FromResult(0);
  });
  ```

`UseStatusCodePages` middleware returns pretty pages for standard status code

`UseStatusCodePagesWithRedirects` middleware
  `app.UseStatusCodePagesWithRedirects("/{0}.html");` redirects the status code to the matching static file
  So a `404` would be redirected to `404.html`

`UseStatusCodePagesWithReExecute` middleware
  `app.UseStatusCodePagesWithReExecute("/{0}.html");` reexecutes the request for the matching static file
    This changes the original status code to `200`
  `app.UseStatusCodePagesWithReExecute("/Controller");` reexecutes the request for the specified route
    This preserves the original HTTP status code
  `app.UseStatusCodePagesWithReExecute("/Controller/{0}");` reexecutes the request for the specified route and passes the status code on the request path
    This preserves the original HTTP status code
    And you can flow the status code all the way to the view

Why did the status code change with the static file approach?
  The static files middleware changes the status code whereas the MVC middleware doesn't

Logging using `ILoggerFactory`
  You can update your controller to specify `ILogger` as a dependency
  Then you can log messages using that logger

`HttpContext.Features`
  `HttpContext.Features.Get<IStatusCodeReExecuteFeature>()` to get an object that allowed him to get the original path

Used packagesearch-staging.azurewebsites.net to search for a package

You can start an app in VS using the command line by selecting the app name in the debug drop down

Added support Serilog logging
  https://serilog.net/
  Added the Serilog middleware to the request pipeline
  Didn't have to update any consumers of ILogger
  Serilog dashboard can be installed to view events

Configuration

In the `web.config` file, you can set `stdoutLogEnabled` to `true` and then set `stdoutLogFile` to the file to log to so the console logger will write to that file

`ConfigurationBuilder` is a key/value pair based configuration system

`SetBasePath` restricts the path for locating config files to the provided path

`env.EnvironmentName` can be used to get the current environment's name
  `IHostingEnvironment` is an abstraction for the current hosting environment
  Looks for the `ASPNETCORE_ENVIRONMENT` environment variable

Loading a second configuration file will overwrite any keys that are have already been loaded

Deployments

`dotnet publish`
  Publishes your project
  `dotnet publish -o ../output`
  Takes the source code representation project and creates a structure that you can effectively copy and paste over to a server to run

You can use `dotnet run MyApp.dll` to run your published app

The .NET Core Installer can be used to install the .NET Core runtime so you can run apps

The `web.config` in the root of the project includes as `aspNetCore` module that IIS uses to know how to host your ASP.NET Core apps
  The `web.config` file is rewritten with the necessary settings so that IIS knows how to call `dotnet` to run your app's dll

When publishing, the output is made as portable as possible
  In the `runtimes` folder, you'll find dlls that are specific to the platform you're running on

You can also publishing your app for a specific platform
  Your output will include an exe and everything you need to run your app (including .NET Core itself)














### Publishing ASP.NET Core Applications

https://channel9.msdn.com/Events/dotnetConf/2016/Publishing-ASPNET-Core-Applications



### Modernize ASP.NET Web Forms with Visual Studio 2015

https://channel9.msdn.com/Events/dotnetConf/2016/Modernize-ASPNET-Web-Forms-with-Visual-Studio-2015



### Building Secure Web APIs with ASP.NET Core

https://channel9.msdn.com/Events/dotnetConf/2016/Building-Secure-Web-APIs-with-ASPNET-Core



### Deploying ASP.NET Core applications using Docker Containers

https://channel9.msdn.com/Events/dotnetConf/2016/Deploying-ASPNET-Core-applications-using-Docker-Containers



### The State of .NET (.NET Conf Day 3 Keynote)

https://channel9.msdn.com/Events/dotnetConf/2016/NET-Conf-Day-3-Keynote-Scott-Hanselman



### Building AllReady: Saving Lives with ASP.NET Core

https://channel9.msdn.com/Events/dotnetConf/2016/Building-AllReady-Saving-Lives-with-ASPNET-Core















## Getting Started with F# on .NET Core

https://channel9.msdn.com/Events/Build/2016/T661





## Docker and ASP.NET Core: A Webcast

https://wildermuth.com/2016/03/28/Docker_and_ASP_NET_Core_A_Webcast







## BUILD 2016


### .NET Overview

https://channel9.msdn.com/Events/Build/2016/B891?ocid=player



### ASP.NET Core Deep Dive into MVC

https://channel9.msdn.com/Events/Build/2016/B812



### Deploying ASP.NET Core Applications

https://channel9.msdn.com/Events/Build/2016/B811?ocid=player



### Interview with the ASP.NET Team

https://channel9.msdn.com/Events/Build/2016/C912?ocid=player



### EF Core

https://channel9.msdn.com/Events/Build/2016/B852?ocid=player



### Code Labs

https://github.com/Microsoft-Build-2016/CodeLabs-WebDev



### ASP.NET Core Overview

https://channel9.msdn.com/Events/Build/2016/B810

Visual Studio 15 Preview
  Installs in 1:20... about time!
  This will help make it easier to get people into .NET development with VS

Lay of the land
  ASP.NET Core spans both .NET Core and .NET Framework
  RTM - ASP.NET 4.6, .NET Framework 4.6, .NET Framework libraries, Compilers and runtime components
  OSS - ASP.NET 4.6, ASP.NET Core 1.0, .NET Core 1.0, .NET core libraries, Compilers and runtime components

ASP.NET 5 (now Core)
  Totally modular
  Seamless transition form on-prem to cloud
  Open source with contributions
  Faster development cycle
  Choose your editors and tools
  Cross platform
  Fast

SignalR and Web Pages support will eventually come

How do you like your ASP.NET?
  You can choose how "rare" you want your platform

OmniSharp brings intellisense to other editors as a service

Benchmarks
  Web benchmarks https://www.techempower.com/benchmarks/
  Hello world app runs fast ??? million requests per second
  Orchard port to ASP.NET Core experienced a 10x performance improvement

ASP.NET Core is modular
  Throw an exception and you won't see an error page unless you're configured that middleware

`wwwroot` is the root of your web app
  This is where files are served from

You can tailor the middleware for specific routes/paths in your app

The files in your project is now based on the file system

ASP.NET Core embraces NPM and Bower to get front end and/or tooling dependencies
  NuGet is still used for .NET dependencies

Improved views with Tag Helpers
  Razor and MVC HTML helpers made it difficult to work with front end frameworks
  Now your HTML can look like HTML

There's a team working on the Windows Console
  Command Line, Power Shell, and Bash all benefit from that work

Docker support
  In Visual Studio you can right click on a project and select "Add Docker Support"
  Remote debugging is enabled in Visual Studio

`dotnet publish` allows you to publish to a specific platform
  For instance, from Windows, publish to Mac OS X and then run that app from a USB key

























## Questions

What loggers does .NET Core or ASP.NET Core support?
  Console logger (logs to the console... does it only work if you're running from the console?)
  Debug logger (logs to the debug console... is that the Output window or something else?)
  Others???

When you referece a method (i.e. `app.UseStaticFiles()`) that requires a new dependency to be added...
  Does VS Code give you a shortcut to add that dependency like VS proper does?

Is there still an IIS native loader?
  I thought that Kestrel was the only app server?

What are the specific parts of the project.json file?

What is the difference between the project.json when using VS Code and VS proper?

What are the files that are generated into the bin folder?

What are the files that VS Code adds to your project?
  How do these compare to the files that VS proper requires?

What does it take to share projects between VS and VS Code

Should you use project.json `dependencies` as much as possible over framework `dependencies`?

Is there just one .dll now for .NET Core?
  Did there used to be multiple .dlls?

If you build and deploy an ASP.NET Core app that targets .NET Framework...
  Do you get access to things that aren't available in .NET Core?
  For instance, WCF?








## TODO



Read up on HttpContext.Features
  This looks like a collection of request/response features???
  How does middleware work with this collection???






Create new Windows 10 VM (using Virtual Box or Fusion?)
  Install VS Community and Update 3

Try using Docker from my Surface Book
  Use that machine for showing a demo of Docker???





Test using VS 15 Preview
  https://blogs.msdn.microsoft.com/visualstudio/2016/08/22/visual-studio-15-preview-4/

Test publishing a simple app using `dotnet publish`
  Can I publish to a local IIS???
    What does it take to get an ASP.NET Core app up and running on IIS???
  Can I publish to Azure???
  Can I get an app running in Docker???

What does it take to publish a class library to NuGet???

Test using Bash on Windows

Practice using the Windows zoom tool

Practice using the VS tools for running Gulp (or webpack?) tasks
  Do NPM scripts also show up in the task runner???

Build my own tag helper

Experiment with multiple desktops in Windows 10

Try using Docker on Windows
  https://docs.docker.com/docker-for-windows/

Review the source code for the MVC methods that are used to configure services and middleware for MVC
  What does it take to add in source code for a dependency???

Test using dotnet watch
  https://github.com/aspnet/dotnet-watch

Check out VS extension that grabs a dependency's source code
  How difficult would it be to create a command line version of this?

Review the source code for the methods that are used to create a web app

Look into how to set environment variables from the command line and from VS









ASP.NET Core and .NET Core Overview
https://weblog.west-wind.com/posts/2016/Jun/13/ASPNET-Core-and-NET-Core-Overview

First Steps: Exploring .NET Core and ASP.NET Core
https://weblog.west-wind.com/posts/2016/Jun/29/First-Steps-Exploring-NET-Core-and-ASPNET-Core

Debug Dockerized .NET Core Apps with VS Code
http://www.bloggedbychris.com/2016/08/03/debug-dockerized-net-core-apps-code/?utm_source=twitterfeed&utm_medium=facebook

Exploring the cookie authentication middleware in ASP.NET Core
http://andrewlock.net/exploring-the-cookieauthenticationmiddleware-in-asp-net-core/

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







## .NET Core at BUILD 2016 - Scott Hunter, Seth Juarez, ​Richard Lander, Beth Massi

https://channel9.msdn.com/Events/Build/2016/C924

Internal organization

The ASP.NET team asked themselves...
  Why wouldn't you want to use .NET?
    Performance
    Cross platform
    Open source
  Then let's knock'em down

Meanwhile... the .NET team is working on Windows Universal (UWP)
  Once that project was done, they decided to work with the ASP.NET team to reconcile DNX back into .NET
  Needed to genericise DNX into the dotnet CLI so that it could work with all types of development

TFM - Target Framework Moniker

The teams are still working on how to clearly communicate when something is not ready for consumption (i.e. experimental)

DNX was...
  A set of APIs that you could target (i.e. a shared framework)
  A set of command line tools
  A set of runtime services and host

The dotnet CLI...
  Still has those concepts but you can pick and chose what you use

So... the bottom line
  The previous bits were ASP.NET specific
  The new bits are designed to encompass all types of .NET development

How does the .NET Standard library and the .NET Foundation relate to one another?
  There not really related at all
  .NET Standard Library is a technology concept
  .NET Foundation is an organization

The .NET Standard Library is an API spec
  It's really just a piece of paper listing what APIs are available for a given version of .NET
  .NET Core and .NET Framework will implement various .NET Standard Library versions
  When you create a library, you'll be able to specify which .NET Standard Library version it was designed to run on
  Then by extension your library will run all of the platforms that support that .NET Standard Library version
  Similiar to Portable Class Library (PCL)... this is the next step beyond that
    PCL was an intersection of the selected platforms which resulted in a tiny slice of the API
      "Happenstance" API
    The goal of .NET Standard is to widen it out
      There's a set of people who are thinking about what APIs should be supported across all platforms
      "Curated" API

Will .NET Core ever become compatible with .NET Framework 4.6?
  There are three layers...
    Runtime layer
    Base class library layer
    App model layer
  .NET Core already has the runtime layer implemented
  There's some work to do in the BCL layer
  In the app model layer you have things like WPF, WinForms, ASP.NET Web Forms
    They'll stay on .NET Framework
    It would be really difficult to port these app models to other platforms
  .NET Framework is delivered with Windows
  .NET Core and Xamarin are delivered with the app

.NET will be developed as one product, one code base
  There is conditional compilation
  And there's a platform abstraction shim that helps hide the specific platform details from .NET

Will SOAP and MSMQ support be coming to .NET Core?
  Too early to tell
  Don't expect that WCF will be migrated wholesale to .NET Core
  But maybe some part of it in order to support SOAP services
    Or maybe they add SOAP capabilities to Web API






















## ASP.NET Core and .NET Core Overview

https://weblog.west-wind.com/posts/2016/Jun/13/ASPNET-Core-and-NET-Core-Overview

Three major platform updates
  Project K (ASP.NET vNext) - KRE, KVM, KPM
  DNX, DNVM, DNU
  .NET Core and ASP.NET Core

.NET Core and ASP.NET Core have RTM'd
  New features will be coming soon
  But MS is telling us that the current API won't drastically change

.NET Standard Library
  The goal is to have a unified base layer for all platforms will support
  .NET Core today gives you an idea of what the future will bring

Should you build apps using ASP.NET Core today???
  It depends on what you're doing
  Some third party stuff is not compatible















## Smallest Web App

1) Run the `dotnet new` command.

2) Add `"Microsoft.AspNetCore.Server.Kestrel": "1.0.0"` as a dependency.

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

## Smallest MVC App

Starting from the "Smallest Web App" build...

1) Add `"Microsoft.AspNetCore.Mvc": "1.0.0"` as a dependency.

2) Open `Program.cs` and remove the following namespaces.

```
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;
```

Then change the `Main` method to the following.

```
new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Build()
    .Run();
```

3) Add `Startup.cs` to the root of the project.

```
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;

namespace SmallestMvcApp
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
        }

        public void Configure(IApplicationBuilder app)
        {
            app.UseMvcWithDefaultRoute();
        }
    }
}
```

4) Add a "Controllers" folder.

5) Add `HomeController.cs` to the "Controllers" folder.

```
public class HomeController
{
    public string Index()
    {
        return "Hello from the controller!";
    }
}
```

Now let's add a view!

1) Update the `project.json` file.

```
"buildOptions": {
  "emitEntryPoint": true,
  "preserveCompilationContext": true
}
```

2) Open `Program.cs` add the following namespace.

```
using System.IO;
```

Then change the `Main` method to the following.

```
new WebHostBuilder()
    .UseKestrel()
    .UseContentRoot(Directory.GetCurrentDirectory()) // This is necessary to support views.
    .UseStartup<Startup>()
    .Build()
    .Run();
```

3) Update the controller.

```
using Microsoft.AspNetCore.Mvc;

public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```

4) Add a "Views/Home" folder.

5) Add `Index.cshtml` to the "Views/Home" folder.

```
<h1>Hello from an MVC view!</h1>
```

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
