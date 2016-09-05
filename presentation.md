
# Presentation


## TODO

What's my strategy if I'm not online???
  Have completed versions of my demos???

Practice using the Windows zoom tool





## Outline

Give overview of ASP.NET Core
  What is it?
  Why was it created?

Start with Windows?
  This is what most people will be familiar with

Creating a new project
  Show VS first
  Build confidence that they haven't moved the cheese (not completely)  
  We can even deploy this to Azure
  Show that adding an action method and view just works as you expect it to

The basic things are more or less the same
  But the platform has undergone a major overhaul
  Let's create a simple app and build it up from there

Create a project using the dotnet CLI
  Start simple and build it up

Mac OS X
  Setup your env (slides)
  Create a new console app
  Turn it into a basic web app
  Turn it into a basic MVC app
  Show debugging in VS Code

Windows
  Start with VS Community with Update 3 installation
  Install .NET Core tools
    Talk through these steps (slides)
  Show the experience of creating a new project
    Pretty much what you're used to

Features to show
  Error handling
  Static files
  Default files
  Middleware
  Adding MVC to an empty project
  EF Core
  Logging
  Configuration
  Built-in DI
  Combined MVC and Web API surface
  Tag helpers
  View components?

Debugging into source code

Things to come...
  Final tooling
    .csproj is coming back!
    But it won't suck!
  SignalR
  Web Pages
  JavaScript Services





## Ideas

Give quick overview of what ASP.NET Core is and why it was created
  Why Project K was started
    Benefits and goals
  The evolution to DNX
  The change to .NET Core






Create a new project using command line on Windows
  Show dotnet CLI commands
  Start with creating console app
    Show what's in the project?
  Show using VS Code on Windows
  Convert the console app to a web app
  Update the web app to an MVC app
  Dig into how middleware works
  Dig into how services work

OR

Create a new project using VS
  Show the two different project types
  What is different between targeting .NET Core or the .NET Framework?





Send the project over to the Mac for the designer to update
  Show how the project and data layer can be used on a Mac
  Show how tag helpers can help designers feel more at home with working in MVC views
  Show how front end devs can leverage the tools that they know
    npm for front-end package management
    webpack for file bundling
      Linting, TypeScript compilation, unit testing, etc.

Send the project back to Windows for the dev to use VS to complete the API implementation
  Show how to use JavaScript services to run a Node.js service on the server
  Deploy the project to Azure Web App?

Show how to create a project on the Mac
  Show using the `dotnet` CLI
    Limited templates at this point
  Show using Yeoman
    More options exist here

Let's remind ourselves what an ASP.NET 4.6 project looks like
  Poll the audience...
    Who uses...
      Empty project?
      Empty project with MVC?
      MVC project?
      With or without auth?
  Review what the project structure looks like
    Two web.configs
      When was the last time that you looked closely at the contents of these web.config files?
      What parts of the web.config files are really needed?
      What does each section do?
    Assembly refs... which are really needed?
    What is needed to configure an MVC app?
      Route config?
  What is IIS providing for us?
    Default files?
    Static files?
    Anything else?

Now...
  Let's compare that with creating an ASP.NET Core project
  Create a console project first
  Then convert the project into a web app
  Then add support for MVC

Middleware overview
  Create 2-3 examples of arbitrary custom middleware to show how the request flows down and up through the middleware
  Use source code for existing ASP.NET Core middleware and set breakpoints in those methods to show how the execution flows???









