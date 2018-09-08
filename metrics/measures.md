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

**Method:** `MeasureDouble` _&&_ `MeasureLong`

**Arguments:**

1. **`name`** \(_string_\): A string by which the measure will be referred to. Names MUST be unique within the library. It is recommended to use names compatible with the intended end usage, e.g, use host/path pattern. _Example:_ `"compressor_app/size"`
2. **`description`** \(string\): A string describing the measure. _Example:_ ****`"Size of the video in bytes"`.
3. **`unit`** \(string\): A string describing the unit used for the `Measure`. Follows the format described by [Unified Code for Units of Measure](http://unitsofmeasure.org/ucum.html). _Example:_ `"By"`
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

**Method:** `stats.Int64` _&&_ `stats.Float64`

**Arguments:**

1. **`name`** \(_string_\): A string by which the measure will be referred to. Names MUST be unique within the library. It is recommended to use names compatible with the intended end usage, e.g, use host/path pattern. _Example:_ `"compressor_app/size"`
2. **`description`** \(string\): A string describing the measure. _Example:_ ****`"Size of the video in bytes"`.
3. **`unit`** \(string\): A string describing the unit used for the `Measure`. Follows the format described by [Unified Code for Units of Measure](http://unitsofmeasure.org/ucum.html). _Example:_ `"By"`
{% endtab %}

{% tab title="Node.js" %}
```javascript
import { Stats, MeasureUnit } from "@opencensus/core";

// Our Stats manager
const stats = new Stats();

const mLatencyMs = stats.createMeasureDouble("compressor_app/latency", MeasureUnit.MS, "The latency of the method in milliseconds");
const mSizeBy = stats.createMeasureInt64("compressor_app/size", MeasureUnit.BYTE, "The size of the video in bytes");
```

**Method:** `Stats.createMeasureDouble` _&&_ `Stats.createMeasureInt64`

**Arguments:**

1. **`name`** \(_string_\): A string by which the measure will be referred to. Names MUST be unique within the library. It is recommended to use names compatible with the intended end usage, e.g, use host/path pattern. _Example:_ `"compressor_app/size"`
2. **`unit`** \(string\): A string describing the unit used for the `Measure`. Follows the format described by [Unified Code for Units of Measure](http://unitsofmeasure.org/ucum.html). _Example:_ `"By"`
3. **`description`** \(string\): A string describing the measure. _Example:_ ****`"Size of the video in bytes"`
{% endtab %}

{% tab title="Python" %}
```python
from opencensus.stats import measure as measure_module
from opencensus.stats import stats as stats_module

m_size_by = measure_module.MeasureInt("compressor_app/size", "The size of the video in bytes", "By");
m_latency_ms = measure_module.MeasureFloat("compressor_app/latency", "The latency of the method in milliseconds", "ms")
```

**Method:** `Measure.MeasureInt` _&&_ `Measure.MeasureFloat`

**Arguments:**

1. **`name`** \(_string_\): A string by which the measure will be referred to. Names MUST be unique within the library. It is recommended to use names compatible with the intended end usage, e.g, use host/path pattern. _Example:_ `"compressor_app/size"`
2. **`description`** \(string\): A string describing the measure. _Example:_ ****`"Size of the video in bytes"`.
3. **`unit`** \(string\): A string describing the unit used for the `Measure`. Follows the format described by [Unified Code for Units of Measure](http://unitsofmeasure.org/ucum.html). _Example:_ `"By"`
{% endtab %}
{% endtabs %}

