# Redis

### Introduction {#introduction}

Some Redis clients were already instrumented to provide traces and metrics with OpenCensus

| PACKAGES | REPOSITORY LINK |
| :--- | :--- |
| jedis | [https://github.com/opencensus-integrations/jedis](https://github.com/opencensus-integrations/jedis) |

### Prerequisites {#prerequisites}

You will need the following:

* Redis
* Google Stackdriver enabled on your project

For assistance installing Redis, please [Click here to get started](https://redis.io/topics/quickstart)

For assistance setting up Stackdriver, [Click here](https://opencensus.io/codelabs/stackdriver) for a guided codelab.

### Generating the JAR {#generating-the-jar}

#### Clone this repository {#clone-this-repository}

```text
git clone https://github.com/opencensus-integrations
```

#### Generate and install {#generate-and-install}

Inside the cloned repository’s directory run

```text
mvn install:install-file -Dfile=$(pwd)/target/jedis-3.0.0-SNAPSHOT.jar \
-DgroupId=redis.clients -DartifactId=jedis -Dversion=3.0.0 \
-Dpackaging=jar -DgeneratePom=true
```

### Enabling observability {#enabling-observability}

To enable observability, we’ll need to use Jedis normally but with one change

```text
import redis.clients.jedis.Observability;
```

and then finally to enable metrics

```text
// Enable exporting of all the Jedis specific metrics and views
Observability.registerAllViews();
```

### Available metrics {#available-metrics}

| METRIC SEARCH SUFFIX | DESCRIPTION |
| :--- | :--- |
| redis/bytes\_read | The number of bytes read from the Redis server |
| redis/bytes\_written | The number of bytes written out to the Redis server |
| redis/dials | The number of connection dials made to the Redis server |
| redis/dial\_latency\_milliseconds | The number of milliseconds spent performing Redis operations |
| redis/errors | The number of errors encountered |
| redis/connections\_opened | The number of new connections |
| redis/roundtrip\_latency | The latency spent for various Redis operations |
| redis/reads | The number of reads performed |
| redis/writes | The number of writes performed |

### End to end example {#end-to-end-example}

* [SOURCE](https://opencensus.io/guides/integrations/redis/java/#0)
* [POM](https://opencensus.io/guides/integrations/redis/java/#1)

```java
package io.opencensus.tutorials.jedis;

import io.opencensus.exporter.stats.stackdriver.StackdriverStatsConfiguration;
import io.opencensus.exporter.stats.stackdriver.StackdriverStatsExporter;
import io.opencensus.exporter.trace.stackdriver.StackdriverTraceConfiguration;
import io.opencensus.exporter.trace.stackdriver.StackdriverTraceExporter;
import io.opencensus.trace.Tracing;
import io.opencensus.trace.config.TraceConfig;
import io.opencensus.trace.config.TraceParams;
import io.opencensus.trace.samplers.Samplers;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.Observability;

public class JedisOpenCensus {
    private static final Jedis jedis = new Jedis("localhost");

    public static void main(String ...args) {
        // Enable exporting of all the Jedis specific metrics and views.
        Observability.registerAllViews();

        // Now enable OpenCensus exporters
        setupOpenCensusExporters();

        // Now for the repl
        BufferedReader stdin = new BufferedReader(new InputStreamReader(System.in));

        while (true) {
            try {
                System.out.print("> ");
                System.out.flush();
                String query = stdin.readLine();

                // Check Redis if we've got a hit firstly
                String result = jedis.get(query);
                if (result == null || result == "") {
                    // Cache miss so process it and memoize it
                    result = "$" + query + "$";
                    jedis.set(query, result);
                }
                System.out.println("< " + result + "\n");
            } catch (IOException e) {
                System.err.println("Exception "+ e);
            }
        }
    }

    private static void setupOpenCensusExporters() {
        String gcpProjectId = "census-demos";

        try {
            StackdriverTraceExporter.createAndRegister(
                StackdriverTraceConfiguration.builder()
                .setProjectId(gcpProjectId)
                .build());

            StackdriverStatsExporter.createAndRegister(
                StackdriverStatsConfiguration.builder()
                .setProjectId(gcpProjectId)
                .build());
        } catch (Exception e) {
            System.err.println("Failed to setup OpenCensus " + e);
        }

        // Change the sampling rate to always sample
        TraceConfig traceConfig = Tracing.getTraceConfig();
        traceConfig.updateActiveTraceParams(
                traceConfig.getActiveTraceParams().toBuilder().setSampler(Samplers.alwaysSample()).build());
    }
}
```

#### Running it {#running-it}

```text
mvn install && mvn exec:java -Dexec.mainClass=io.opencensus.tutorials.jedis.JedisOpenCensus
```

### Viewing your metrics {#viewing-your-metrics}

Please visit [https://console.cloud.google.com/monitoring](https://console.cloud.google.com/monitoring)

### Viewing your traces {#viewing-your-traces}

Please visit [https://console.cloud.google.com/traces/traces](https://console.cloud.google.com/traces/traces)

