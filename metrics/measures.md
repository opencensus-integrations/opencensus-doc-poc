# Measures

Measures allow you to record a quantifiable value. Declaring your measures is usually the first step in capturing Metrics.

{% hint style="success" %}
**Example:** You want to record the byte count of videos to be processed in your video compression service.
{% endhint %}

### **Usage**

Measures can be used to record an `int64` or a `double`.

{% tabs %}
{% tab title="Java" %}
```java
import io.opencensus.stats.Measure;
import io.opencensus.stats.Measure.MeasureLong;
import io.opencensus.stats.Measure.MeasureDouble;

public class App {
    private static final MeasureLong VIDEO_SIZE =
        MeasureLong.create("compressor_app/size", "Size of the video in bytes", "by");
    
    private static final MeasureDouble RPC_LATENCY =
        MeasureDouble.create("compressor_app/latency", "Latency of method in milliseconds", "ms");
}
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method:</b>  <code>MeasureDouble.create</code>  <em>&&</em>  <code>MeasureLong.create</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the measure
            will be referred to. Names MUST be unique within the library. It is recommended
            to use names compatible with the intended end usage, e.g, use host/path
            pattern. <em>Example:</em>  <code>&quot;compressor_app/size&quot;</code>
          </li>
          <li><b><code>description</code></b> (string): A string describing the measure. <em>Example:</em><b> </b><code>&quot;Size of the video in bytes&quot;</code>.</li>
          <li><b><code>unit</code></b> (string): A string describing the unit used for
            the <code>Measure</code>. Follows the format described by <a href="http://unitsofmeasure.org/ucum.html">Unified Code for Units of Measure</a>. <em>Example:</em>  <code>&quot;By&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Go" %}
```go
import (
	"go.opencensus.io/stats"
)

var (
	MSizeBy = stats.Int64("compressor_app/size", "Size of the video in bytes", "By")
    MLatencyMs = stats.Float64("compressor_app/latency", "Latency of method in milliseconds ", "ms")
)
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>stats.Int64</code>  <em>&&</em>  <code>stats.Float64</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the measure
            will be referred to. Names MUST be unique within the library. It is recommended
            to use names compatible with the intended end usage, e.g, use host/path
            pattern. <em>Example:</em>  <code>&quot;compressor_app/size&quot;</code>
          </li>
          <li><b><code>description</code></b> (string): A string describing the measure. <em>Example:</em><b> </b><code>&quot;Size of the video in bytes&quot;</code>.</li>
          <li><b><code>unit</code></b> (string): A string describing the unit used for
            the <code>Measure</code>. Follows the format described by <a href="http://unitsofmeasure.org/ucum.html">Unified Code for Units of Measure</a>. <em>Example:</em>  <code>&quot;By&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Node.js" %}
```javascript
import { Stats, MeasureUnit } from "@opencensus/core";

// Our Stats manager
const stats = new Stats();

const mLatencyMs = stats.createMeasureDouble("compressor_app/latency", MeasureUnit.MS, "The latency of the method in milliseconds");
const mSizeBy = stats.createMeasureInt64("compressor_app/size", MeasureUnit.BYTE, "The size of the video in bytes");
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>Stats.createMeasureDouble</code>  <em>&&</em>  <code>Stats.createMeasureInt64</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the measure
            will be referred to. Names MUST be unique within the library. It is recommended
            to use names compatible with the intended end usage, e.g, use host/path
            pattern. <em>Example:</em>  <code>&quot;compressor_app/size&quot;</code>
          </li>
          <li><b><code>unit</code></b> (string): A string describing the unit used for
            the <code>Measure</code>. Follows the format described by <a href="http://unitsofmeasure.org/ucum.html">Unified Code for Units of Measure</a>. <em>Example:</em>  <code>&quot;By&quot;</code>
          </li>
          <li><b><code>description</code></b> (string): A string describing the measure. <em>Example:</em><b> </b><code>&quot;Size of the video in bytes&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Python" %}
```python
from opencensus.stats import measure as measure_module
from opencensus.stats import stats as stats_module

m_size_by = measure_module.MeasureInt("compressor_app/size", "The size of the video in bytes", "By");
m_latency_ms = measure_module.MeasureFloat("compressor_app/latency", "The latency of the method in milliseconds", "ms")
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method:</b>  <code>Measure.MeasureInt</code>  <em>&& </em><code>Measure.MeasureFloat</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the measure
            will be referred to. Names MUST be unique within the library. It is recommended
            to use names compatible with the intended end usage, e.g, use host/path
            pattern. <em>Example:</em>  <code>&quot;compressor_app/size&quot;</code>
          </li>
          <li><b><code>description</code></b> (string): A string describing the measure. <em>Example:</em><b> </b><code>&quot;Size of the video in bytes&quot;</code>.</li>
          <li><b><code>unit</code></b> (string): A string describing the unit used for
            the <code>Measure</code>. Follows the format described by <a href="http://unitsofmeasure.org/ucum.html">Unified Code for Units of Measure</a>. <em>Example:</em>  <code>&quot;By&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}
{% endtabs %}

