# gRPC

gRPC are automatically traced by OpenCensus. All you need to is to configure one of the [OpenCensus Trace Exporters](https://saturnism.gitbook.io/opencensus/~/drafts/-LLefzbTo_CgoPI0SwdY/primary/exporters).

When you make a client gRPC call to a gRPC server, the OpenCensus trace context is automatically propagated.

gRPC are automatically instrumented by OpenCensus. All you need to is to configure one of the [OpenCensus Exporters](../../exporters/), or [zPages](../../zpages.md) to see the data.

gRPC is a high performance, open-source universal RPC framework. OpenCensus is integrated with gRPC in the following languages:

| Language |  |  |
| :--- | :--- | :--- |
| [Java](java.md) | Tracing | Metrics |
| [Go](go.md) | Tracing | Metrics |
| [Python](python.md) | Tracing |  |
| [C++](c++.md) | Tracing | Metrics |

