# Tracing

A trace tracks the progression of a single user request as it is handled by other services that make up an application.

Each unit work is called a span in a trace. Spans include metadata about the work, including the time spent in the step \(latency\) and status. You can use tracing to debug errors and latency issues of your application.

A trace is just a tree of Spans.

![A trace with an error span](https://opencensus.io/img/trace-errorspan.png)

## **Sampling** 

Trace data is often very large in size and is expensive to collect. This is why rather than collecting traces for every request, downsampling is prefered. By default, OpenCensus provides a probabilistic sampler that will trace once in every 10,000 requests.

See [OpenCensus Tracing Sampling](sampling.md) to learn how to configure different sampling rates.

## **Exporting** 

Recorded spans will be reported by the registered exporters.

Multiple exporters can be registered to upload the data to various different backends. Users can unregister the exporters if they no longer are needed.

