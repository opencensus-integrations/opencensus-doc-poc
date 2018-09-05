# Zipkin

Zipkin is a distributed tracing system. It helps gather timing data needed to troubleshoot latency problems in microservice architectures.

It manages both the collection and lookup of this data. Zipkin’s design is based on the Google Dapper paper.

OpenCensus Java has support for this exporter available through package [io.opencensus.exporter.trace.zipkin](https://www.javadoc.io/doc/io.opencensus/opencensus-exporter-trace-zipkin)  


## Configure Maven / Gradle

{% tabs %}
{% tab title="Maven" %}
```markup
<dependency>
    <groupId>io.opencensus</groupId>
    <artifactId>opencensus-exporter-trace-zipkin</artifactId>
    <version>${opencensus.version}</version>
</dependency>
```
{% endtab %}

{% tab title="Gradle" %}

{% endtab %}
{% endtabs %}

## Configure Exporter

{% tabs %}
{% tab title="Snippet" %}
```java
ZipkinTraceExporter.createAndRegister(
    "http://localhost:9411/api/v2/spans",
    "service-a");
```
{% endtab %}

{% tab title="All" %}
```java
package io.opencensus.tutorial.zipkin;

import io.opencensus.exporter.trace.zipkin.ZipkinTraceExporter;

public class ZipkinTutorial {
    public static void main(String ...args) throws Exception {
        ZipkinTraceExporter.createAndRegister("http://localhost:9411/api/v2/spans", "service-a");
    }
}
```
{% endtab %}
{% endtabs %}

The `createAndRegister` method takes in 2 parameters:

| Parameter | Description |
| :--- | :--- |
| URL | ... |
| Service Name | .. |

## Example

A full working example in [opencensus-tracing-to-zipkin](https://github.com/saturnism/opencensus-java-by-example/tree/master/opencensus-tracing-to-zipkin) GitHub repository. 

1. Clone the example repository: `git clone https://github.com/saturnism/opencensus-java-by-example`
2. Change to the example directory: `cd opencensus-java-by-example/opencensus-tracing-to-zipkin`
3. Download Zipkin: `curl -sSL https://zipkin.io/quickstart.sh | bash -s`
4. Start Zipkin: `java -jar zipkin.jar`
5. Run the code: `mvn compile exec:java -Dexec.mainClass=com.example.TracingToZipkin`
6. Navigate to Zipkin Web UI: `http://localhost:9411`
7. Click _Find Traces_, and you should see a trace.
8. Click into that, and you should see the details.

![](../../../.gitbook/assets/image%20%281%29.png)

