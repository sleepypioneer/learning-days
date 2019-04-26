## Instrumenting Python application with Prometheus

https://github.com/prometheus/client_python

* Set to `/metrics` endpoint

`elif endpoint == '/metrics':`  
`return super(HTTPRequestHandler, self).do_GET()`



* Instead of [BaseHTTPRequest](https://docs.python.org/2/library/basehttpserver.html) use MetricsHandle](https://github.com/prometheus/client_python/blob/3cb4c9247f3f08dfbe650b6bdf1f53aa5f6683c1/prometheus_client/exposition.py#L141) wrapper class

* May hit issues with HTTP 1:1 : https://github.com/prometheus/client_python/issues/299

#### Counter
Counters go up, and reset when the process restarts.

```from prometheus_client import Counter
c = Counter('my_failures', 'Description of counter')
c.inc()     # Increment by 1
c.inc(1.6)  # Increment by given value```

https://github.com/prometheus/client_python#counter


#### labels

... Labels can also be passed as keyword-arguments:

```from prometheus_client import Counter
c = Counter('my_requests_total', 'HTTP Failures', ['method', 'endpoint'])
c.labels(method='get', endpoint='/').inc()
c.labels(method='post', endpoint='/submit').inc()```

https://github.com/prometheus/client_python#labels

`c = Counter('requests_total', 'requests', ['status', 'endpoint'])`
