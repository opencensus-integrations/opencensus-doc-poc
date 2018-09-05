# Sampling

Trace data is often very large in size and is expensive to collect. This is why rather than collecting traces for every request, downsampling is preferred. By configuring the sampler and sampling rate, you can control how much trace data to export to the backend.

## Global and Span Sampler

You can configure the sampler \(and thus the sampling rate\) globally, or for a specific span.

* **Global sampler**: Global sampler is the global default.
* **Span sampler**: When starting a new span, a custom sampler can be provided. If no custom sampling is provided, the global sampler is used. Span samplers are useful if you want to over-sample some sections of your code. For example, a low throughput background service may use a higher sampling rate than a high-load RPC server.

### Global Sampler

Global sampler is configured in `TraceConfig`.

```java
		// Configure a Global Sampler
		TraceConfig traceConfig = Tracing.getTraceConfig();
		TraceParams activeTraceParams = traceConfig.getActiveTraceParams();

		// Example A: Configure 0% sample rate.
		traceConfig.updateActiveTraceParams(
			activeTraceParams.toBuilder().setSampler(
				Samplers.alwaysSample()).build());
```

### Span Sampler

When starting a new span, a custom sampler can be provided. If no custom sampling is provided, the global sampler is used. Span samplers are useful if you want to over-sample some sections of your code. For example, a low throughput background service may use a higher sampling rate than a high-load RPC server.

```java
		try (Scope scope = tracer.spanBuilder("main")
				.setSampler(Samplers.alwaysSample())
				.startScopedSpan()) {
			...
		}
```

### Which Sampler Will Be Used?

The OpenCensus library samples based on the following rules:

1. If the span is a root `Span`, then a `Sampler` will be used to make the sampling decision:
   * If a "span-scoped" `Sampler` is provided, use it to determine the sampling decision.
   * Else use the global default `Sampler` to determine the sampling decision.
2. If the span is a child of a remote `Span` the sampling decision will be:
   * If a "span-scoped" `Sampler` is provided, use it to determine the sampling decision.
   * Else use the global default `Sampler` to determine the sampling decision.
3. If the span is a child of a local `Span` the sampling decision will be:
   * If a "span-scoped" `Sampler` is provided, use it to determine the sampling decision.
   * Else keep the sampling decision from the parent.

## Sampling Rate

Whether you configure the globally or just for the span, you can use one of the following pre-created samplers to configure the sampling rate:

* `AlwaysSample` - sampler that makes a "yes" decision every time.
* `NeverSample` - sampler that makes a "no" decision every time.
* `Probability` - sampler that tries to uniformly sample traces with a given probability. When applied to a child `Span` of a **sampled** parent `Span`, the child `Span` keeps the sampling decision.

By default, OpenCensus uses a probabilistic sampler that will sample one in every 10,000 requests.

### Always Sampler

```java
Samplers.probabilitySampler(0.01)
```

### Never Sampler

```java
Samplers.neverSample()
```

### Probability Sampler

```java
Samplers.probabilitySampler(0.01)
```





#### 

#### 

#### 

