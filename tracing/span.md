# Span

A span is the unit work represented in a trace. A span may represent a HTTP request, an RPC, a server handler, a database query or a section  marked in user code.

For example:

![A trace](https://opencensus.io/img/trace-trace.png)

Above, you see a trace with various spans. In order to respond to `/messages`, several other internal requests are made. Firstly, we check if the user is authenticated. Next we check if their messages were cached. Since their message wasn’t cached, that’s a cache miss and we then fetch their content from MySQL, cache it and then provide the response containing their messages.

A span may or may not have a parent span:

* A span without a parent is called a **“root span”** for example span “/messages”
* A span with a parent is called a **“child span”** for example spans “auth”, “cache.Get”, “mysql.Query”, “cache.Put”

Spans are identified with a SpanID and each span belongs to a single trace. Each trace is uniquely identified by a TraceID which all constituent spans will share.

These identifiers and options byte together are called **Span Context**. Inside the same process, **Span context**is propagated in a context object. When crossing process boundaries, it is serialized into protocol headers. The receiving end can read the **Span context** and create child spans.

## **Name**

Span names are symbolic of the what span does. Span names should be statistically meaningful. Most tracing backend and analysis tools use span names to auto generate reports for the represented work.

Examples of span names:

* “cache.Get” represents the Get method of the cache service.
* ”/messages” represents the messages web page.
* ”/api/user/\(\d+\)” represents the user detail pages.

## Creating a Span

You can use Tracer to create a new Span.

```java
Tracer tracer = Tracing.getTracer();
try (Scope scope = tracer.spanBuilder("work").startScopedSpan()) {
   ...
}
```

This code snippet will create a new Span, however, whether it's a root span or a child span depends on whether you already have another overarching Span.

This span has "work" as the Span Name.

## Getting Current Span

You may need to access the current Span in order to enhance it with additional information such as:

* [Annotation](annotation.md)
* [Status](status.md)

To retrieve the Span from the current context.

```java
Tracer tracer = Tracing.getTracer();
tracer.getCurrentSpan()

// Do something with the Span
span.addAnnotation("doing work");
```



