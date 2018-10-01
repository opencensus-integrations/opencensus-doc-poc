# Status

Status represents the current state of the span. It is represented by a canonical status code which maps onto a predefined set of error values and an optional string message.

Status allows tracing visualization tools to highlight unsuccessful spans and helps tracing users to debug errors.

![A trace with an error span](https://opencensus.io/img/trace-errorspan.png)

Above, you can see `cache.Put` errored the request violated the preset key size limit. As a result of this error,`/messages` request responded with an error to the user.

## Setting Status

```java
// 1. Get the current Span from the context
Span span = tracer.getCurrentSpan();

// 2. Set the Status with the exception as the description.
span.setStatus(Status.OK.withDescription("Everything is fine"));
```

## Handling Errors

It's more common that when you handle errors in your code, you also  propagate the error as the Span Status.

```java
try (Scope scope = tracer.spanBuilder("main").startScopedSpan()) {
	try {
		throw new RuntimeException(("oops"));
	} catch (Exception e) {
		// 1. Get the current Span from the context
		Span span = tracer.getCurrentSpan();

		// 2. Set the Status with the exception as the description.
		span.setStatus(Status.INTERNAL
			.withDescription(e.toString()));
	}
}
```

See the canonical [OpenCensus Status Codes](status.md#status-codes) for all of the possible statuses you can use.

## Status Codes

<table>
  <thead>
    <tr>
      <th style="text-align:left">Status Code</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">OK</td>
      <td style="text-align:left">The operation completed successfully.</td>
    </tr>
    <tr>
      <td style="text-align:left">CANCELLED</td>
      <td style="text-align:left">The operation was cancelled (typically by the caller).</td>
    </tr>
    <tr>
      <td style="text-align:left">UNKNOWN</td>
      <td style="text-align:left">Unknown error. An example of where this error may be returned is if a
        Status value received from another address space belongs to an error-space
        that is not known in this address space. Also errors raised by APIs that
        do not return enough error information may be converted to this error.</td>
    </tr>
    <tr>
      <td style="text-align:left">INVALID_ARGUMENT</td>
      <td style="text-align:left">Client specified an invalid argument. Note that this differs from FAILED_PRECONDITION.
        INVALID_ARGUMENT indicates arguments that are problematic regardless of
        the state of the system (e.g., a malformed file name).</td>
    </tr>
    <tr>
      <td style="text-align:left">DEADLINE_EXCEEDED</td>
      <td style="text-align:left">Deadline expired before operation could complete. For operations that
        change the state of the system, this error may be returned even if the
        operation has completed successfully. For example, a successful response
        from a server could have been delayed long enough for the deadline to expire.</td>
    </tr>
    <tr>
      <td style="text-align:left">NOT_FOUND</td>
      <td style="text-align:left">Some requested entity (e.g., file or directory) was not found.</td>
    </tr>
    <tr>
      <td style="text-align:left">ALREADY_EXISTS</td>
      <td style="text-align:left">Some entity that we attempted to create (e.g., file or directory) already
        exists.</td>
    </tr>
    <tr>
      <td style="text-align:left">PERMISSION_DENIED</td>
      <td style="text-align:left">The caller does not have permission to execute the specified operation.
        PERMISSION_DENIED must not be used for rejections caused by exhausting
        some resource (use RESOURCE_EXHAUSTED instead for those errors). PERMISSION_DENIED
        must not be used if the caller cannot be identified (use UNAUTHENTICATED
        instead for those errors).</td>
    </tr>
    <tr>
      <td style="text-align:left">RESOURCE_EXHAUSTED</td>
      <td style="text-align:left">Some resource has been exhausted, perhaps a per-user quota, or perhaps
        the entire file system is out of space.</td>
    </tr>
    <tr>
      <td style="text-align:left">FAILED_PRECONDITION</td>
      <td style="text-align:left">
        <p>Operation was rejected because the system is not in a state required for
          the operation's execution. For example, directory to be deleted may be
          non-empty, an rmdir operation is applied to a non-directory, etc.</p>
        <p></p>
        <p>A litmus test that may help a service implementor in deciding between
          FAILED_PRECONDITION, ABORTED, and UNAVAILABLE:</p>
        <p></p>
        <p>(a) Use UNAVAILABLE if the client can retry just the failing call.</p>
        <p>(b) Use ABORTED if the client should retry at a higher-level (e.g., restarting
          a read-modify-write sequence). (c) Use FAILED_PRECONDITION if the client
          should not retry until the system state has been explicitly fixed. E.g.,
          if an "rmdir" fails because the directory is non-empty, FAILED_PRECONDITION
          should be returned since the client should not retry unless they have first
          fixed up the directory by deleting files from it.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ABORTED</td>
      <td style="text-align:left">The operation was aborted, typically due to a concurrency issue like sequencer
        check failures, transaction aborts, etc.</td>
    </tr>
    <tr>
      <td style="text-align:left">OUT_OF_RANGE</td>
      <td style="text-align:left">
        <p></p>
        <p>Operation was attempted past the valid range. E.g., seeking or reading
          past end of file.</p>
        <p></p>
        <p>Unlike INVALID_ARGUMENT, this error indicates a problem that may be fixed
          if the system state changes. For example, a 32-bit file system will generate
          INVALID_ARGUMENT if asked to read at an offset that is not in the range
          [0,2^32-1], but it will generate OUT_OF_RANGE if asked to read from an
          offset past the current file size.</p>
        <p></p>
        <p>There is a fair bit of overlap between FAILED_PRECONDITION and OUT_OF_RANGE.
          We recommend using OUT_OF_RANGE (the more specific error) when it applies
          so that callers who are iterating through a space can easily look for an
          OUT_OF_RANGE error to detect when they are done.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">UNIMPLEMENTED</td>
      <td style="text-align:left">Operation is not implemented or not supported/enabled in this service.</td>
    </tr>
    <tr>
      <td style="text-align:left">INTERNAL</td>
      <td style="text-align:left">Internal errors. Means some invariants expected by underlying system has
        been broken. If you see one of these errors, something is very broken.</td>
    </tr>
    <tr>
      <td style="text-align:left">UNAVAILABLE</td>
      <td style="text-align:left">
        <p>The service is currently unavailable. This is a most likely a transient
          condition and may be corrected by retrying with a backoff.</p>
        <p></p>
        <p>See litmus test above for deciding between FAILED_PRECONDITION, ABORTED,
          and UNAVAILABLE.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">DATA_LOSS</td>
      <td style="text-align:left">Unrecoverable data loss or corruption.</td>
    </tr>
    <tr>
      <td style="text-align:left">UNAUTHENTICATED</td>
      <td style="text-align:left">The request does not have valid authentication credentials for the operation.</td>
    </tr>
  </tbody>
</table>