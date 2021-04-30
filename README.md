
## run the containers

```
docker-compose up -d ;   
docker logs otel-prometheus-reproducer_otel_1  -f
```

At the moment this docker-compose won't work because the `otel ` container fails to locate the `promethapp` endpoint. The `theproxy` container, on the other hand, can proxy requests:

```
# promethapp is available in port 9001
curl http://localhost:9001

# hitting the proxy retuns the payload served from promethapp
curl -H"Host: theproxy.example.com"  http://localhost:9999
```

`otel` container logs include:

```
2021-04-30T11:08:36.194Z	info	service/service.go:205	Starting processors...
2021-04-30T11:08:36.194Z	info	builder/pipelines_builder.go:51	Pipeline is starting...	{"pipeline_name": "metrics", "pipeline_datatype": "metrics"}
2021-04-30T11:08:36.194Z	info	builder/pipelines_builder.go:62	Pipeline is started.	{"pipeline_name": "metrics", "pipeline_datatype": "metrics"}
2021-04-30T11:08:36.194Z	info	service/service.go:210	Starting receivers...
2021-04-30T11:08:36.194Z	info	builder/receivers_builder.go:70	Receiver is starting...	{"kind": "receiver", "name": "prometheus"}
2021-04-30T11:08:36.195Z	info	builder/receivers_builder.go:75	Receiver started.	{"kind": "receiver", "name": "prometheus"}
2021-04-30T11:08:36.195Z	info	healthcheck/handler.go:128	Health Check state change	{"kind": "extension", "name": "health_check", "status": "ready"}
2021-04-30T11:08:36.195Z	info	service/application.go:201	Everything is ready. Begin running and processing data.
2021-04-30T11:08:42.064Z	warn	internal/metricsbuilder.go:104	Failed to scrape Prometheus endpoint	{"kind": "receiver", "name": "prometheus", "scrape_timestamp": 1619780922060, "target_labels": "map[instance:172.16.238.10:80 job:akka-prometh-otel]"}
2021-04-30T11:08:44.065Z	warn	internal/metricsbuilder.go:104	Failed to scrape Prometheus endpoint	{"kind": "receiver", "name": "prometheus", "scrape_timestamp": 1619780924060, "target_labels": "map[instance:172.16.238.10:80 job:akka-prometh-otel]"}
```



