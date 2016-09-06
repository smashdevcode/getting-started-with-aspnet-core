
# Demos

## TODO

* Review demos for more ideas and/or detail to cover here
* Practice with no internet connectivity
* Practice using the Windows zoom tool

## File > New Project

* Show Windows + VS first
* Build confidence that they haven't moved the cheese (not completely)  
* Show that adding an action method and view just works as you expect it to
* We can even deploy this to Azure if we wanted to

## Simple App

The basic things are more or less the same.

* But the platform has undergone a major overhaul
* Let's create a simple app and build it up from there

Create a project using the .NET CLI.

* Start simple and build it up
* Create a new console app
* Turn it into a basic web app (no startup file)

## Middleware

Middleware replaces HttpModules and HttpHandlers in IIS.

[Open the Middleware demo project]

* Show how to create middleware
* Show how the pipeline works

### Exploring Built-In Middleware

We don't have to create all of our own middleware of course. Let's explore some of the major built-in middleware.

Let's create a new empty project using Visual Studio.

[Create new empty project using Visual Studio]

TODO Just walk through the middleware that is part of the project template???

* Logging
 * Generic logging wrapper around whatever providers you want to configure
* Configuration
 * Show JSON config file
 * Show how to access config values
 * Can be customized
* Error handling (status code pages)
 * Show how to 
* Static files
 * Explain the purpose of `wwwroot`
 * Add a static file and try to browse to it
* Default files
 * No longer set in IIS (for ASP.NET Core apps)

## Using MVC

Now let's add MVC to our project.

* Adding MVC
* Add POCO controller
* Add MVC controller and view
* Add API controller

## JSON.net Changes

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

## Built-in DI

Dependency injection is pervasive throughout ASP.NET Core.

* Add a "Repository" class
* Add it as a service
* Inject it into the controller

## Tag Helpers

Instead of using HTML helper methods, which aren't very friendly for people who aren't familiar with Razor, we can now use Tag Helpers.

* Show form using HTML helpers
* Show the same form with Tag Helpers

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

[Switch to Mac...]

## Debugging in VS Code

We can easily move our projects from Windows to the Mac and vice versa.

Entity Framework database providers and database migrations can complicate things, but it is possible to work it out so that you can support developers on macOS (or Linux).

## Yeoman

More templates then the .NET CLI.

* Show generating a project using Yeoman

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
* Show the differences in the `project.json`
* Overall minor differences
* Important to note that the app is still built using the `dotnet` CLI
* Also still hosted using Kestrel with IIS as a reverse proxy
