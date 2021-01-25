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
        requestUri: "http://localhost:5000",
        batchFormatter: new Serilog.Sinks.Http.BatchFormatters.ArrayBatchFormatter(),
        queueLimit:10)
.WriteTo.Console()
.CreateLogger();
```

# The issue


```json
{
  "_index": "logstash-2021.01.25-000001",
  "_type": "_doc",
  "_id": "Lj5NOXcBTZs4VCBDHeca",
  "_score": 1,
  "_source": {
    "port": 37070,
    "@timestamp": "2021-01-25T11:28:10.903Z",
    "message": ",{\"Timestamp\":\"2021-01-25T12:28:06.1609791+01:00\",\"Level\":\"Information\",\"MessageTemplate\":\"Get\",\"RenderedMessage\":\"Get\",\"Properties\":{\"SourceContext\":\"FooApi.Controllers.WeatherForecastController\",\"ActionId\":\"c11badc3-5c00-4b8f-bd03-894f4a677c69\",\"ActionName\":\"FooApi.Controllers.WeatherForecastController.Get (FooApi)\",\"RequestId\":\"0HM614H4FNOIE:00000002\",\"RequestPath\":\"/weatherforecast\",\"ConnectionId\":\"0HM614H4FNOIE\"}}\r",
    "@version": "1",
    "host": "gateway"
  },
  "fields": {
    "@timestamp": [
      "2021-01-25T11:28:10.903Z"
    ]
  }
}
```

 - message property contain

```text
,{\"Timestamp\":\"2021-01-25T12:28:06.1609791+01:00\",\"Level\":\"Information\",\"MessageTemplate\":\"Get\",\"RenderedMessage\":\"Get\",\"Properties\":{\"SourceContext\":\"FooApi.Controllers.WeatherForecastController\",\"ActionId\":\"c11badc3-5c00-4b8f-bd03-894f4a677c69\",\"ActionName\":\"FooApi.Controllers.WeatherForecastController.Get (FooApi)\",\"RequestId\":\"0HM614H4FNOIE:00000002\",\"RequestPath\":\"/weatherforecast\",\"ConnectionId\":\"0HM614H4FNOIE\"}}\r
```

 - unescape message is

```text
,{"Timestamp":"2021-01-25T12:28:06.1609791+01:00","Level":"Information","MessageTemplate":"Get","RenderedMessage":"Get","Properties":{"SourceContext":"FooApi.Controllers.WeatherForecastController","ActionId":"c11badc3-5c00-4b8f-bd03-894f4a677c69","ActionName":"FooApi.Controllers.WeatherForecastController.Get (FooApi)","RequestId":"0HM614H4FNOIE:00000002","RequestPath":"/weatherforecast","ConnectionId":"0HM614H4FNOIE"}}
```