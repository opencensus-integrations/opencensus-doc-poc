# Java with Jedis

There is a specialized Jedis client that implements OpenCensus Tracing and Metrics. \[TODO: This is not the best way to integrate\]

|  | REPOSITORY LINK |
| :--- | :--- |
| jedis | [https://github.com/opencensus-integrations/jedis](https://github.com/opencensus-integrations/jedis) |

## Configure Maven / Gradle

First, Install the artifact.

```bash
git clone ... jedis-opencensus
cd jedis-opencensus
mvn install
mvn install:install-file -Dfile=target/jedis-3.0.0-SNAPSHOT.jar \
  -DgroupId=redis.clients -DartifactId=jedis -Dversion=3.0.0-opencensus \
  -Dpackaging=jar -DgeneratePom=true
```

Then, configure the build dependency for Maven or Gradle.

{% tabs %}
{% tab title="Maven" %}
```markup
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.0.0-opencensus</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>
```
{% endtab %}

{% tab title="Gradle" %}
```text

```
{% endtab %}
{% endtabs %}

## Enable OpenCensus

Use `redis.clients.jedis.Observability` to enable observability and register all traces and metrics with OpenCensus.

```java
// Enable exporting of all the Jedis specific metrics and views
Observability.registerAllViews();
```

## Example

...

