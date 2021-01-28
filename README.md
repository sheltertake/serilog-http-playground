# serilog-http-playground

 - startup elastic stack


```cmd
cd elastic-stack
docker-compose up
```

 - navigate kibana http://localhost:5601/
 - login elastic/changeme
 - setup index pattern
 - navigate discover

```cmd
cd src
dotnet run
```

 - navigate http://localhost:5001/weatherforecast

# The api configuration

```csharp
 Log.Logger = new LoggerConfiguration()
.MinimumLevel.Is(LogEventLevel.Debug)
.MinimumLevel.Override("Microsoft", LogEventLevel.Warning)
.MinimumLevel.Override("System", LogEventLevel.Warning)
.WriteTo.Http(
        requestUri: "http://localhost:31311",
        batchFormatter: new Serilog.Sinks.Http.BatchFormatters.ArrayBatchFormatter(),
        queueLimit:10)
.WriteTo.Console()
.CreateLogger();
```
