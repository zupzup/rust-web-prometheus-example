# rust-web-prometheus-example

An example for using Prometheus Metrics in a Rust Web Service

Build prometheus with:

```bash
cd prom
docker build -t prometheus
```

Run it with:

```bash
docker run -p 9090:9090 --network=host prometheus
```

Run service with `make dev`.

Go to http://localhost:9090/graph and enter the following commands to see the quantiles of `response_time`:

```bash
histogram_quantile(0.50, sum(rate(response_time_bucket{env="testing"}[2m])) by (le))
histogram_quantile(0.90, sum(rate(response_time_bucket{env="testing"}[2m])) by (le))
histogram_quantile(0.99, sum(rate(response_time_bucket{env="testing"}[2m])) by (le))

```

And the rate of increase for response codes by Status Type:

```bash
sum(increase(response_code{env="testing"}[5m])) by (type)
```

You can also call the `/some` endpoint and observer the `incoming_requests` counter, as well as connect via websockets at `/ws/some_id` and observe the `connected_clients` counter.
