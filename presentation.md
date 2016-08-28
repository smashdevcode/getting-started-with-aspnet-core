
# Presentation

Ideas

Give quick overview of what ASP.NET Core is and why it was created

Create a new project using command line on Windows
  Show dotnet CLI commands
  Show using VS Code on Windows
  Include simple data model and EF data store?

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










