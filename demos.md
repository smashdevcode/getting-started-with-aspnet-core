
# Demos

## Prep

* Disable power saving features
* Turn off notifications
* Open Powershell, set font size, and browse to the desktop folder

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

Snippet: _sau

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

Snippet: _sah

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

Snippet: _sc

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
            ...
        }
    }
}
```

Your application will "use" middleware.

* Compose your request pipeline using middleware
* Middleware perform asynchronous logic on an HttpContext
* Optionally invoke the next middleware in the sequence or terminate the request directly
* Use extension methods on IApplicationBuilder to configure middleware
* Show how to create middleware using `Run` and `Use`
* Show how the pipeline works by setting breakpoints and running the app
* Middleware replaces HttpModules and HttpHandlers in IIS

### Custom Middleware

Having our middleware implementation directly in the `Startup.cs` file is not really a best practice. Let's look at an example of moving our custom middleware to its own file.

Snippet: _cm

```
using System.Threading.Tasks; 
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

namespace Middleware.Middleware
{
    public class CustomMiddleware
    {
        private readonly RequestDelegate _next;
        private readonly string _name;

        public CustomMiddleware(RequestDelegate next, string name)
        {
            _next = next;
            _name = name;
        }

        public async Task Invoke(HttpContext context)
        {
            await context.Response.WriteAsync($"<p>{_name} Middleware!</p>");
            await _next.Invoke(context);
        }
    }

    public static class CustomMiddlewareExtensions
    {
        public static IApplicationBuilder UseCustomMiddleware(
            this IApplicationBuilder app, string name = "Custom")
        {
            return app.UseMiddleware<CustomMiddleware>(name);
        }
    }
}
```

## Creating an MVC Project Using File > New Project

You don't have to use the command line if you don't want to.

Let's start with a new empty project using Visual Studio.

[Create new empty project using Visual Studio]

Notice that packages are restoring here in the Solution Exploring.

Now let's build our app. Notice in the Output window that Visual Studio is using the same `dotnet build` command that we were using a minute ago.

### Exploring Built-In Middleware

We don't have to create all of our own middleware of course. Let's explore some of the major built-in middleware.

#### Configuration

* Show JSON config file
* Show how to access config values
* Can be customized

#### Logging

* `ILoggerFactory` is what we use to configure logging in our apps
* Generic logging wrapper around whatever providers you want to configure

We can get an instance of a logger by using the logger factory's `CreateLogger` method.

```
var logger = loggerFactory.CreateLogger<Startup>();
logger.LogInformation("Hello from Startup!");
```

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

* Services are components intended for common consumption in your application
* Services are made available through dependency injection
* Three varieties of services
 * Singleton (one instance)
 * Scoped (per HTTP request)
 * Transient (per container request)

3) Inject it into the POCO controller

[Run the app and show the content]

If the built-in DI container doesn't work for your app, then you can replace it with something else. Ninject, Autofac, etc.

## API Controllers

Let's take a look at API controllers in ASP.NET Core.

[Open API controllers demo project]

* Web API has now been merged into MVC
* MVC and API controllers now share the same base class
* We can now use the `[controller]` and `[action]` tokens in our routes
* We need to be explicit with the verbs that our methods should respond to
* You can now provide the route template in the HTTP verb attributes

### JSON.net Changes

Now defaults to camel-case for your property names.

[Use POSTMAN to show JSON response]

Show how to fix this.

Snippet: _jo

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

[Open tag helpers project]

Let's start with looking a simple form for "contacts". This is a pretty basic MVC Razor form. Nothing surprising here.

```
@model TagHelpers.Models.Contact

@{
    ViewData["Title"] = "Add Contact";
}

<h2>@ViewData["Title"].</h2>

@using (Html.BeginForm())
{
    <div class="form-group">
        @Html.LabelFor(m => m.Name, new { @class ="control-label" })
        @Html.TextBoxFor(m => m.Name, new { @class = "form-control" })
    </div>

    <div class="form-group">
        @Html.LabelFor(m => m.Email, new { @class = "control-label" })
        @Html.TextBoxFor(m => m.Email, new { @class = "form-control" })
    </div>

    <div class="form-group">
        @Html.LabelFor(m => m.Phone, new { @class = "control-label" })
        @Html.TextBoxFor(m => m.Phone, new { @class = "form-control" })
    </div>

    <div>
        <button class="btn btn-lg btn-success">Save</button>
    </div>
}
```

Now let's look at the Tag Helpers version. Notice any differences?

```
@model TagHelpers.Models.Contact

@{
    ViewData["Title"] = "Add Contact";
}

<h2>@ViewData["Title"].</h2>

<form method="post">
    <div class="form-group">
        <label asp-for="Name" class="control-label"></label>
        <input asp-for="Name" class="form-control" />
    </div>

    <div class="form-group">
        <label asp-for="Email" class="control-label"></label>
        <input asp-for="Email" class="form-control" />
    </div>

    <div class="form-group">
        <label asp-for="Phone" class="control-label"></label>
        <input asp-for="Phone" class="form-control" />
    </div>

    <div>
        <button class="btn btn-lg btn-success">Save</button>
    </div>
</form>
```

Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.

* An HTML-friendly development experience
* A rich IntelliSense environment for creating HTML and Razor markup
* A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server

Let's also take a look at how Tag Helpers are being used in the layout page.

[Open the layout page and review tag helpers in it]

## Custom Tag Helpers

You can also create custom tag helpers

* Add a custom tag helper
* Add the tag helper to our form

```
using Microsoft.AspNetCore.Razor.TagHelpers;

namespace TagHelpers.TagHelpers
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
@addTagHelper *, TagHelpers
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

## Finding Packages

Visual Studio Code, at this time, doesn't provide any help to locate the packages that you need. If you're having trouble finding something, then try this website.

https://packagesearch.azurewebsites.net/

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
