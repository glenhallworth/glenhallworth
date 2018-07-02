Title: appsettings.json in .NET Core Console Apps
Published: 7/3/2018
Tags: [".NET Core", "Console", "appsettings.json", "Configuration"]

---

When creating a .NET Core console application, it's handy to have Web API style appsettings.json to configure the application.

```json
{
  "AppConfig": {
    "DatabaseConnectionString":
      "Data source=(LocalDb)\\MSSQLLocalDB;Initial Catalog=Dev;Integrated Security=True"
  }
}
```

It would be nice to bind this to a concrete class.

```csharp
    class AppConfig
    {
        public string DatabaseConnectionString { get; set; }
    }
```

1.  Install NuGet packages

    - Microsoft.Extensions.Configuration
    - Microsoft.Extensions.Configuration.Binder
    - Microsoft.Extensions.Configuration.Json

2.  Add an extension method to IConfiguration

    ```csharp
    public static class ConfigurationExtensions
        {
            public static T ParseAs<T>(this IConfiguration configuration, string path)
                where T : new()
            {
                var result = new T();
                configuration.GetSection(path).Bind(result);
                return result;
            }
        }
    ```

3.  Use the configuration in your program

    ```csharp
        class Program
        {
            static void Main(string[] args)
            {
                var configuration = GetStandardConfigurationBuilder();
                var appConfig = configuration.ParseAs<AppConfig>("AppConfig");
                Console.WriteLine(appConfig.DatabaseConnectionString);
            }

            internal static IConfiguration GetStandardConfigurationBuilder()
            {
                return new ConfigurationBuilder()
                    .AddJsonFile("appsettings.json").Build();
            }
        }
    ```
