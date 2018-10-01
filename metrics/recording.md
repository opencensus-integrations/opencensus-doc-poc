# Recording

When you are have the desired metrics and tags and want to make them available for export, you will then record the information. 

{% hint style="success" %}
**Example**: After the client device has been tagged and the video size is known, you can record the measure and tags using a measurement object.
{% endhint %}

### Usage

{% tabs %}
{% tab title="Java" %}
```java
import io.opencensus.stats.Measure;
import io.opencensus.stats.Measure.MeasureLong;
import io.opencensus.common.Scope;
import io.opencensus.stats.StatsRecorder;
import io.opencensus.tags.TagKey;
import io.opencensus.tags.TagValue;

public class App {
  private static final TagKey KEY_DEVICE = TagKey.create("device");
  private static final Tagger tagger = Tags.getTagger();
  
  private static final MeasureLong VIDEO_SIZE =
      MeasureLong.create("compressor_app/size", "Size of the video in bytes", "by");
      
  // 1. Initialize the Recorder
  private static final StatsRecorder statsRecorder = Stats.getStatsRecorder();
    
  public static void main(String ...args) {
    try (Scope scopedTags =
      tagger
        .currentBuilder()
        .put(KEY_DEVICE, TagValue.create("mobile-ios")
        .buildScoped())) {
      // 2. Record a measure
      // Note: Automatically captures the "mobile-ios" tag
      statsRecorder.newMeasureMap().put(VIDEO_SIZE, 5000000).record();
    }
}
```

| **Method**: `StatsRecorder.newMeasureMap` |
| :--- |
| Returns an object for recording multiple measurements. |
| Arguments: \(none\) |

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>MeasureMap.put</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Associates the <code>Measure</code> (<code>double</code> or <code>long</code>)
        with the given value. Subsequent updates to the same <code>Measure</code> will
        overwrite the previous value.</td>
    </tr>
    <tr>
      <td style="text-align:left">Arguments:</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <ol>
          <li><code>measure</code> (<em>MeasureDouble<b> </b></em><b>| </b><em>MeasureLong)</em>:
            Measure to record. <em>Example: </em><code>MeasureLong.create(&quot;compressor_app/size&quot;, &quot;Size of the video in bytes&quot;, &quot;by&quot;);</code>
          </li>
          <li><code>value</code> (<em>Double </em>| <em>Long</em>): Value to be associated
            with the <code>Measure</code>. <em>Example: </em><code>50000</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>: <code>MeasureMap.record</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Records all of the measures at the same time, with the current or explicit<code>TagContext</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li>tag context (<em>optional</em>) (<em>TagContext</em>): The tags associated
            with the measurements. Example: <code>statsRecord.record(new SimpleTagContext(Tag.create(KEY, VALUE)))</code>
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
	"go.opencensus.io/tag"
	"go.opencensus.io/stats"
)
​
var (
	KeyDevice, _ = tag.NewKey("device")
	MSizeBy = stats.Int64("compressor_app/size", "Size of the video in bytes", "By")
)
​
func main() {
	ctx, err := tag.New(context.Background(), tag.Insert(KeyDevice, "mobile-ios"))
​
	// 1. Record the Measure
	// Note: Automatically captures the "mobile-ios" tag
	stats.Record(ctx, MSizeBy.M(5000000))
}
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>Int64Measure.M </code><em><code>&amp;&amp; </code></em><code>Float64Measure.M</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Creates a new measurement to be later recorded.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>value</code> (<em>int64</em> | <em>float64</em>): Value to associate
            with the measurement. <em>Example: <code>500000</code></em>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>: <code>stats.record</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Records one or multiple measures with the same tags at once.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>context</code> (<em>context.Context</em>): The context associated
            with the measurement. <em>Example:</em>  <code>context.Background()</code>
          </li>
          <li><code>measurement</code> (...<em>Measurement</em>): The measurements to
            record. <em>Example:</em>  <code>MSizeBy.M(5000000)</code>
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

// Our Video Size Measure (bytes)
const mSizeBy = stats.createMeasureInt64("compressor_app/size", MeasureUnit.BYTE, "The size of the video in bytes");

// Our Tag
const tagDevice = "device";

// 1. Records the video size in bytes
stats.record({
  measure: mSizeBy,
  tags: {tagDevice: "mobile-ios"},
  value: 5000000
});
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>Stats.record</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Updates all views with the new measurements.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol></ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="Python" %}
```python
from opencensus.stats import measure as measure_module
from opencensus.stats import stats as stats_module
from opencensus.tags import tag_key as tag_key_module
from opencensus.tags import tag_map as tag_map_module
from opencensus.tags import tag_value as tag_value_module
​
m_size_by = measure_module.MeasureInt("compressor_app/size", "The size of the video in bytes", "By");
m_latency_ms = measure_module.MeasureFloat("compressor_app/latency", "The latency of the method in milliseconds", "ms")​

# 1. The stats recorder
stats_recorder = stats_module.Stats().stats_recorder
​
# Create the tag key
key_device = tag_key_module.TagKey("device")
​
def main():
  # 2. Create the measure_map into which we'll insert the measurements
  mmap = stats_recorder.new_measurement_map()
​
  # 3. Record the Video Size
  mmap.measure_int_put(m_size_by, 5000000)
  mmap.measure_float_put(m_lantecy_ms, 1.3)
  ​
  # Create the Tag
  tmap = tag_map_module.TagMap()
  tmap.insert(key_device, tag_value_module.TagValue("mobile-ios"))
  ​
  # 4. Insert the tag map
  mmap.record(tmap)
  ​
if __name__ == "__main__":
   main()
```

| **Method:** `StatsRecorder.new_measurement_map` |
| :--- |
| Creates a new MeasurementMap in order to record stats. |
| Arguments: \(none\) |

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>:<b> </b><code>MeasurementMap.measure_int_put</code>  <em>&& </em><code>MeasurementMap.measure_float_put</code> 
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Associates the measure of type (<em>Float </em>| <em>Int64</em>) with the
        given value.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>measure</code> (<em>MeasureInt</em> | <em>MeasureFloat</em>): The measure
            to associate a value with. <em>Example: </em><code>measure_module.MeasureInt(&quot;compressor_app/size&quot;, &quot;The size of the video in bytes&quot;, &quot;By&quot;)</code>
          </li>
          <li><code>value</code> (<em>Int64</em> | <em>Float</em>): The value to associate
            with the measure. <em>Example: </em><code>5000000</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method:</b> MeasureMap.record</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Records all the measures at the same time with a tag_map. tag_map could
        either be explicitly passed to the method, or implicitly read from current
        execution context.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>tags</code> (<em>TagMap</em>): The tags to associate with the recording. <em>Example: </em><code>tag_map_module.TagMap()</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}
{% endtabs %}

