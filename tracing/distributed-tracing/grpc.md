# gRPC

gRPC are automatically traced by OpenCensus. All you need to is to configure one of the [OpenCensus Trace Exporters](https://saturnism.gitbook.io/opencensus/~/drafts/-LLefzbTo_CgoPI0SwdY/primary/exporters).

When you make a client gRPC call to a gRPC server, the OpenCensus trace context is automatically propagated.

## Example

