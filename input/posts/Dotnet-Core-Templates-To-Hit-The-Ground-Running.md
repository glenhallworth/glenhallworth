Title: Dotnet Core Templates To Hit The Ground Running
Published: 11/17/2017
Tags: [".NET Core", "Templates"]

---

The problem with starting a new project is there is a lot of boilerplate work to get started. You need to setup project structure, source control, build and release pipelines, dependency injection and logging before you write a line of business code. Dotnet Core 2.0 supports custom templates meaningless boilerplate code to hit the ground running.

To test creating custom templates I built one for a common app type - C# Web API backend for a single page application. It can be found https://github.com/glenhallworth/WebApiWithSpa. I've made it generic including several libraries setup how I like and have left the flexible pieces to be added; such as authentication.

The templates are distributed using NuGet so it's incredibly easy to use. To get the above template up and running its just two commands:

1.  Install the template from NuGet: dotnet new -i WebApiWithSpa.Template.CSharp
2.  Create the project: dotnet new WebApiWithSpa

There is not much to the template, the above template only has three parts to it.

- A nuspec file to create a NuGet package.
- A template.json file to config the template.
- The template code files.

```
/
  WebApiWithSpa.nuspec - The nuspec file to pack the NuGet package.
  content/
    .template.config/ - A folder for template configuration.
      template.json - The template configuration.
    WebApiWithSpa/ - The code to use for the project.
```

This saves me time setting up new projects as the template has already been setup with all the common aspects for projects of this type. There are always tasks at the beginning of the project which are set and forget - logging being an example.

The project contains the boilerplate structure of the project:

- _Domain Layer_ - The core business logic of an application. Includes IQuery and ICommand interfaces to support CQRS pattern.
- _Web API Layer_ - The API of the application. Has a sample Values controller supporting example CRUD operations. Also has logging with Serilog and static file serving is setup.
- _Code Gen_ - A console application to generate typescript interfaces for the API request/response objects. Makes writing the SPA easier by automating the creation of the API contract. This builds every time the Web API layer builds.
- _Unit Tests_ - A project for unit tests as every project should have unit tests.

Furthermore, you can add optional content to the template to support toggling features on or off. As a future enhancement, I'll add optional authentication setup.

If you create several .NET projects every year you should consider using templates to save yourself time lowering the barrier to beginning a new project. Incredibly useful if you or your company has a standard project structure and preferred toolset. Not only will you make your project setup quicker because it's so easy to share with other people can benefit from your template.
