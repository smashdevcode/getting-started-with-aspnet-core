
# Notes

## Questions

Is there still an IIS native loader???
  I thought that Kestrel was the only app server???







## Building a Web App with ASP.NET Core - Shawn Wildermuth

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






## Resources

Where's DNVM? Safely running multiple versions of the .NET Core SDK and Tooling with global.json
http://www.hanselman.com/blog/WheresDNVMSafelyRunningMultipleVersionsOfTheNETCoreSDKAndToolingWithGlobaljson.aspx

Exploring dotnet new with .NET Core
http://www.hanselman.com/blog/ExploringDotnetNewWithNETCore.aspx
