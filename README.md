
## Installation

Install my-project with nuget


according to your requirment, you could select one of these jobs:
- `ForeverJob`
- `ScheduledJob`

Build your job with fluent Builder `BackgroundJobBuilder`, if want build `Scheduled` job, just pass your cron to builder with `WithCron` method.

ForeverJob:
```bash
  var forever = new BackgroundJobBuilder()
        .WithName("some-job")
        .WithAction(SomeBackgroundJob.Run, new CancellationToken())
        .WithDependency<ISomeService>(typeof(SomeService))
        .Build();
```


scheduledJob:
```bash
  var scheduledJob = new BackgroundJobBuilder()
        .WithCron("0/15 * * ? * * *")
        .WithName("some-job")
        .WithAction(SomeBackgroundJob.Run, new CancellationToken())
        .WithDependency<ISomeService>(typeof(SomeService))
        .Build();
```

**Background Job Manager**

it prepares a new standalone `IServiceProvider` and automatically registers services that you use in your action's logic to make you unnecessary from registering your services and selecting their lifetime.

so your service must have `IBackgroundJobInputParser` as an argument and use it instead of the default dependency injection of your application. 

all of your jobs which build, you must add them in `BackgroundJobManager`, this stores jobs.


```bash
 void Add(BackgroundJobManifest backgroundJob);
```

after adding jobs in `BackgroundJobManager`, Call the `Sync` method to start the jobs.

```bash
 void Sync();
```


**Dependency Injection**:

you just register `BackgroundJobManager` as singletone to your default `IServiceCollection`


    
