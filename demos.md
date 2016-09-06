
# Demos

## TODO



* Setup code snippets

* Practice with no internet connectivity
 * Can I have completed, restored versions of the demos???




## Prep

* Create new repo
* Clone to both Windows and macOS
* Copy and paste a copy of the demos to the desktops of each machine
* Open the snippets doc in Sublime on each machine

## Creating a Simple App using the .NET CLI

Create a project using the .NET CLI.

* Start simple and build it up
* Create a new console app

```
mkdir SimpleApp
dotnet new
dotnet restore
dotnet run
```

Let's review our project files in Visual Studio Code.

### Program.cs

Not much happening here... this is just a regular old console app.

### project.json

The `project.json` file is being replaced with a `.csproj`, so I'm not going to spend a lot of time explaining the parts of this file. But I do want to highlight some things.

* The `dependencies` property lists the dependencies for our app
 * Each item represents a NuGet package that needs to be restored before the app can be built and ran
 * Items typically have their own dependencies, so this list of dependencies does not represent the full list of NuGet packages that your app depends on.
* The `frameworks` property lists the frameworks that we're targeting, in this case `netcoreapp1.0` which is .NET Core.

This file becomes bigger for ASP.NET Core apps.

## Simple Web App

Now let's create a simple web app from our "Simple App".

1) Add `"Microsoft.AspNetCore.Server.Kestrel": "1.0.0"` as a dependency.

2) Open `Program.cs` and add the following namespaces.

```
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;
```

And add the following code in the `Main` method.

1) First we start with telling the host to use Kestrel.

2) Then we configure the host by calling the `Configure` method which passes our delegate (or anonymous method) an IApplicationBuilder instance.

3) On the app builder we call the `Run` method and supply another delegate that handles the request. This delegate is a very simple middleware component.

4) Then we build and run the host.

```
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(a => a.Run(c => c.Response.WriteAsync("Hello world!")))
    .Build();

host.Run();
```

5) Then `dotnet restore` and `dotnet run` from the command line.

6) Browse to `http://localhost:5000/`

## Middleware

We could continue to add middleware to our `Configure` delegate, but that would get messy real fast. We can replace our `Configure` method call with a call to the `UseStartup` method.

```
var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Build();

host.Run();
```

### The Startup Class

Applications are defined using a public Startup class.

```
namespace SimpleApp
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
        }

        public void Configure(IApplicationBuilder app)
        {
        }
    }
}
```

You "add" services to your application.

* Services are components intended for common consumption in your application
* Services are made available through dependency injection
* Three varieties of services
 * Singleton (one instance)
 * Scoped (per HTTP request)
 * Transient (per container request)

Your application will "use" middleware.

* Compose your request pipeline using middleware
* Middleware perform asynchronous logic on an HttpContext
* Optionally invoke the next middleware in the sequence or terminate the request directly
* Use extension methods on IApplicationBuilder to configure middleware
* Show how to create middleware using `Run` and `Use`
* Show how the pipeline works by setting breakpoints and running the app
* Middleware replaces HttpModules and HttpHandlers in IIS

## Creating an MVC Project Using File > New Project

You don't have to use the command line if you don't want to.

Let's start with a new empty project using Visual Studio.

[Create new empty project using Visual Studio]

Notice that packages are restoring here in the Solution Exploring.

Now let's build our app. Notice in the Output window that Visual Studio is using the same `dotnet build` command that we were using a minute ago.

### Exploring Built-In Middleware

We don't have to create all of our own middleware of course. Let's explore some of the major built-in middleware.

#### Logging

* Generic logging wrapper around whatever providers you want to configure

#### Configuration

* Show JSON config file
* Show how to access config values
* Can be customized

#### Error Handling

* Show what happens is you don't include the developer error page

#### Static Files

* Explain the purpose of `wwwroot`
* Add a static file and try to browse to it

#### Default Files

* No longer set in IIS (for ASP.NET Core apps)

### Controllers and Views

Controllers and views for the most part look and feel like they did before.

* Controllers no longer need to inherit from the `Controller` base class

[Add a POCO controller class]

* Action methods return `IActionResult`
* Adding an action method and view just works as you expect it to

## Built-in DI

Dependency injection is pervasive throughout ASP.NET Core.

1) Add a "Repository" class

[Add the new file via the file system]

2) Add it as a service to the `Startup.cs` file

```
```

3) Inject it into the POCO controller

[Run the app and show the content]

## Adding an API Controller

Web API has now been merged into MVC. MVC and API controllers now share the same base class.


TODO Add a simple model and API controller

TODO List things to highlight about API controllers





### JSON.net Changes

Now defaults to camel-case for your property names.

[Use POSTMAN to show JSON response]

Show how to fix this.

```
services.AddMvc()
        .AddJsonOptions(opt =>
    {
        var resolver  = opt.SerializerSettings.ContractResolver;
        if (resolver != null)
        {
            var res = resolver as DefaultContractResolver;
            res.NamingStrategy = null;  // <<!-- this removes the camelcasing
        }
    });
```











## Tag Helpers


TODO Setup file on the desktop or the talk repo???




Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.

* An HTML-friendly development experience
* A rich IntelliSense environment for creating HTML and Razor markup
* A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server




Instead of using HTML helper methods, which aren't very friendly for people who aren't familiar with Razor, we can now use Tag Helpers.

* Show form using HTML helpers
* Show the same form with Tag Helpers




## Custom Tag Helpers

You can also create custom tag helpers

* Add a custom tag helper
* Add the tag helper to our form

```
using Microsoft.AspNetCore.Razor.TagHelpers;

namespace AspNetCoreResources.TagHelpers
{
    public class EmailTagHelper : TagHelper
    {
        private const string EmailDomain = "smashdev.com";

        // Can be passed via <email mail-to="..." />. 
        // Pascal case gets translated into lower-kebab-case.
        public string MailTo { get; set; }

        public override void Process(TagHelperContext context, TagHelperOutput output)
        {
            output.TagName = "a";    // Replaces <email> with <a> tag

            var address = MailTo + "@" + EmailDomain;
            output.Attributes.SetAttribute("href", "mailto:" + address);
            output.Content.SetContent(address);
        }
    }
}
```

```
@addTagHelper *, AspNetCoreResources
```





## Publishing

Let's use Visual Studio to publish our app.

[Publish the app]

Now let's copy the files over to a memory stick so that we can run this app on the Mac.

[Switch to Mac...]

Now we can run the app using the .NET CLI.

```
dotnet [app].dll
```

## Creating a Project Using Yeoman

On the Mac, we don't have Visual Studio project templates available to us. We could use the .NET CLI, but it only has a single web app template at this point. Instead, let's use Yeoman.

Yeoman is a utility that you install using NPM.

```
npm install -g yo
npm install -g generator-aspnet
```

Now we can create a project.

```
yo aspnet
```

## Debugging in VS Code

We can easily move our projects from Windows to the Mac and vice versa.

Entity Framework database providers and database migrations can complicate things, but it is possible to work it out so that you can support developers on macOS (or Linux).

Let's open our project in Visual Studio code.

```
code .
```

Now let's run and debug our app.

## Finding Packages

Visual Studio Code, at this time, doesn't provide any help to locate the packages that you need. If you're having trouble finding something, then try this website.

https://packagesearch.azurewebsites.net/

[Switch back to Windows...]

## Targeting the .NET Framework

Do you need to use something that's not available in .NET Core?

* System.Drawing
* DataTable and DataSet

Then you can target the full .NET Framework instead of .NET Core.

* Not cross platform... only runs on Windows (unless you want to target Mono)

Show the differences in the `project.json`

[Open HTML report]

* Overall minor differences
* Important to note that the app is still built using the `dotnet` CLI
* Also still hosted using Kestrel with IIS as a reverse proxy
