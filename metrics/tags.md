# Tags

Tags allow you to add metadata to your measures. Declaring tags will allow you to later filter and categorize your measures.

{% hint style="success" %}
**Example**: You want to record what the client's device was when hitting your video compression service. You then filter your video size measures by client device to conclude that requests made from desktop PCs on average include larger file sizes. 
{% endhint %}

### **Usage**

{% tabs %}
{% tab title="Java" %}
```java
import io.opencensus.common.Scope;
import io.opencensus.tags.TagKey;
import io.opencensus.tags.TagValue;

public class App {
  private static final TagKey KEY_DEVICE = TagKey.create("device");
  private static final Tagger tagger = Tags.getTagger();
  
  public static void main(String ...args) {
    try (Scope scopedTags =
      tagger
        .currentBuilder()
        .put(KEY_DEVICE, TagValue.create("mobile-ios")
        .buildScoped())) {
      // recordings taken here will
      // automatically have the "mobile-ios" tag
    }
}

```
{% endtab %}

{% tab title="Go" %}
```go
import (
	"go.opencensus.io/tag"
)

var (
	KeyDevice, _ = tag.NewKey("device")
)

func main() {
	ctx, err := tag.New(context.Background(), tag.Insert(KeyDevice, "mobile-ios"))

	// recordings with `ctx` will
	// automatically have the "mobile-ios" tag
}
```
{% endtab %}

{% tab title="Node.js" %}
_Note:_ We must explicitly pass the tags in to the recording because Node.js does not have implicit tag awareness. This example **also** shows how to make measures and recordings:

```javascript
import { Stats, MeasureUnit } from "@opencensus/core";

// Our Stats manager
const stats = new Stats();

// Our Video Size Measure (bytes)
const mSizeBy = stats.createMeasureInt64("compressor_app/size", MeasureUnit.BYTE, "The size of the video in bytes");

// Our Tag
const tagDevice = "device";

// Records the video size in bytes
stats.record({
  measure: mSizeBy,
  tags: {tagDevice: "mobile-ios"},
  value: 5000000
});
```
{% endtab %}

{% tab title="Python" %}
_Note:_ We must explicitly pass the tags in to the recording because Python does not have implicit tag awareness. This example **also** shows how to make measures and recordings:

```python
from opencensus.stats import measure as measure_module
from opencensus.stats import stats as stats_module
from opencensus.tags import tag_key as tag_key_module
from opencensus.tags import tag_map as tag_map_module
from opencensus.tags import tag_value as tag_value_module

m_size_by = measure_module.MeasureInt("compressor_app/size", "The size of the video in bytes", "By");

# The stats recorder
stats_recorder = stats.Stats().stats_recorder

# Create the tag key
key_device = tag_key_module.TagKey("device")

def main():
  # Create the measure_map into which we'll insert the measurements
  mmap = stats_recorder.new_measurement_map()

  # Record the Video Size
   mmap.measure_int_put(m_size_by, 5000000)

   # Create the Tag
   tmap = tag_map_module.TagMap()
   tmap.insert(key_device, tag_value_module.TagValue("mobile-ios"))

   # Insert the tag map
   mmap.record(tmap)

if __name__ == "__main__":
   main()
```
{% endtab %}
{% endtabs %}

### Arguments

1. **`name`** \(_string_\): A string by which the tag will be referred to. _Example:_ `"device"`

