
## Simulate the prometheus

Use the HTTP in python3 to simulate the Prometheus endpoint (`:9001`) of the application:

```
cd prometheus
python3 -m http.server 9001 &
curl http://localhost:9001
```

## Run the otel-collector


```
docker run --rm \
  -p 13133:13133 -p 14250:14250 -p 14268:14268 \
  -p 55678-55680:55678-55680 -p 8888:8888 -p 9411:9411 \
  -v ${PWD}/otel:/etc/otel-conf \
  --name otelcol otel/opentelemetry-collector:0.25.0 \
  --config /etc/otel-conf/otel-prometheus-scapper.yaml
```

