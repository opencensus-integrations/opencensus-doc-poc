# Java

Some Redis clients were already instrumented to provide traces and metrics with OpenCensus

| PACKAGES | REPOSITORY LINK |
| :--- | :--- |
| jedis | [https://github.com/opencensus-integrations/jedis](https://github.com/opencensus-integrations/jedis) |

## Configure Maven / Gradle

{% tabs %}
{% tab title="Maven" %}
```markup
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>${jedis.version}</version>
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

