
# Code Snippets

## Simple Web App

### Usings

using Microsoft.AspNetCore.Hosting; 
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

### Host

var host = new WebHostBuilder() 
    .UseKestrel()
    .Configure(a => a.Run(c => c.Response.WriteAsync("Hello world!")))
    .Build();

host.Run();

## Middleware

### Startup Class

using Microsoft.AspNetCore.Builder; 
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Http;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app)
    {
        app.Run(async (context) => 
        {
            await context.Response.WriteAsync("<p>Hello from middleware component 1!</p>");
        });
    }
}

### Custom Middleware

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

## API Controllers

### JSON.net Changes

services.AddMvc() 
    .AddJsonOptions(options =>
	{
		var resolver = options.SerializerSettings.ContractResolver;
		if (resolver != null)
		{
			var res = resolver as DefaultContractResolver;
			res.NamingStrategy = null;
		}
	});
