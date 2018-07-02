Title: Windows Service in 2018
Published: 7/2/2018
Tags: [".NET Core", ".NET", "Windows Service", "Topshelf", "Polly", "FluentScheduler"]

---

# Windows Service in 2018

Most people don't write Windows services anymore. We have the cloud. You can write WebJobs or Functions instead. However in the rare case you have to, say a client with an on-premise server room because their internet connection is awful and a need for background jobs, then this is how I did it.

[Topshelf](http://topshelf-project.com/) - To manage the windows service and simplify development.

[FluentScheduler](https://github.com/fluentscheduler/FluentScheduler) - To schedule jobs.

[Polly](http://www.thepollyproject.org/) - To add resilience and fault handling.

[Autofac](https://autofac.org/) - To glue everything together with dependency injection.

[Serilog](https://serilog.net/) and [Seq](https://getseq.net/) - For logging and debugging.

[TLDR: Show me the code](https://github.com/glenhallworth/WindowsService)

## Topshelf

Topshelf makes developing Windows services easy. You develop a console app which you can install as a Windows service. By making testing in development as simple as running a console app, you significantly reduce the feedback loop and ease of development.

The one caveat is it doesn't support .NET Core yet. There is a develop branch with support. I wouldn't run critical production apps off of it yet. Nevertheless, it should be simple to switch once it does by upgrading the Nuget package and changing the target framework from full framework to .NET Core.

## FluentScheduler

I've avoided creating multiple windows services (and thereby many deployment artefacts) by using FluentScheduler.

You create a job by implementing the IJob interface. Here's a simple job:

```csharp
    public class TimerJob : IJob
    {
        public void Execute()
        {
            var timer = new System.Timers.Timer(1000) { AutoReset = true };
            timer.Elapsed += (sender, eventArgs) => Console.WriteLine("It is {0} and all is well", DateTime.Now);
            timer.Start();;
        }
    }
```

You then registry the job by inheriting from Registry class:

```csharp
 class TimerRegistry : Registry
    {
        public TimerRegistry(TimerJob job, AppConfig appConfig)
        {
            if (appConfig.RunTimerJob)
            {
                Schedule(job).WithName(job.GetType().FullName).ToRunNow();
            }
        }
    }
```

The above job runs when the service starts. You can configure when the job runs with a fluent builder. Here's a job that runs every day at 4 AM.

```csharp
Schedule(job).WithName(job.GetType().FullName).ToRunEvery(1).Days().At(4, 0);
```

I'm using one registry class per job to keep it clean and separated.

## Polly

In the real world, errors happen, HTTP calls fail, and you want to handle that. Polly adds a level of resilience allowing retry strategies if failures occur.

```csharp
private async Task ExecuteAsync()
        {
            var policyResult = await Policy
                .Handle<Exception>()
                .RetryAsync(3)
                .ExecuteAndCaptureAsync(async () =>
                {
                    _logger.Information("Attempting to retrieve");
                    return await DoThing();
                });

            if (policyResult.Outcome == OutcomeType.Failure)
            {
                _logger.Error(policyResult.FinalException, "Could not retrieve status");
                return;
            }

            var response = policyResult.Result;
            _logger.Information($"Status is {response}");
        }
```

## Autofac

I've chosen Autofac for dependency injection. You could use any DI framework. Using assembly scanning I registry all the Jobs and Registries.

```csharp
            builder.RegisterAssemblyTypes(TheBackgroundWorker.Assembly)
                .Where(t => t.IsAssignableTo<IJob>()).AsSelf();

            builder.RegisterAssemblyTypes(TheBackgroundWorker.Assembly)
                .Where(t => t.IsSubclassOf(typeof(Registry))).As<Registry>();
```

The registry objects injected into BackgroundWorkers which configure FluentScheduler's JobManager.

```csharp
 public class BackgroundWorkers
    {
        private readonly IEnumerable<Registry> _registries;

        public BackgroundWorkers(IEnumerable<Registry> registries, ILogger logger)
        {
            _registries = registries;
            JobManager.JobStart += info => logger.Information($"Scheduled job {info.Name} started");
            JobManager.JobEnd += info => logger.Information($"Scheduled job {info.Name} finished");
            JobManager.JobException += info =>
                logger.Error(info.Exception, $"An error occurred on scheduled job {info.Name}");
        }

        public void Start()
        {
            JobManager.Initialize(_registries.ToArray());
        }

        public void Stop()
        {
            JobManager.Stop();
        }
    }
```

Finally BackgroundWorkers in called by Topshelf when the service is started and stopped.

```csharp
		   var backgroundWorkers = container.Resolve<BackgroundWorkers>();
            var rc = HostFactory.Run(x =>
            {
                x.Service<BackgroundWorkers>(s =>
                {
                    s.ConstructUsing(name => backgroundWorkers);
                    s.WhenStarted(tc => tc.Start());
                    s.WhenStopped(tc => tc.Stop());
                });
            });
```

## Summary

I have a Windows service that is easy to develop, easy to deploy and easy to add additional jobs to which can be scheduled as required and can be resilient to faults. It is an easy upgrade to .NET Core once Topshelf releases .NET Core version (you can try the preview now). All the sample code which ties all these excellent packages together found at https://github.com/glenhallworth/WindowsService.
