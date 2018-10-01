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

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method:</b>  <code>TagKey.create</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Create a <code>TagKey</code> for adding metadata to measures.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the tag will
            be referred to. <em>Example: </em><code>&quot;device&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>| **Method:** `Tags.getTagger` |
| :--- |
| Returns the `Tagger` for this implementation. |
| Arguments**:** \(none\) |

|  **Method:** `Tagger.currentBuilder` |
| :--- |
| Returns a new builder created from the current TagContext. |
| Arguments**:** \(none\) |

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>TagContextBuilder.put</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Adds the key/value pair regardless of whether the key is present.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>tag key</code></b> (<em>TagKey</em>): key which will be set. <em>Example: </em><code>TagKey.create(&quot;device&quot;)</code>
          </li>
          <li><b><code>tag value</code></b> (<em>TagValue): </em>value to set for the
            given key. <em>Example:</em>  <code>TagValue.create(&quot;mobile-ios&quot;)</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p></p>
        <p><b>Method:</b>  <code>TagContextBuilder.buildScoped</code>
        </p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Enters the scope of code where the TagContext created from this builder
        is in the current context and returns an object that represents that scope.
        The scope is exited when the returned object is closed.</td>
    </tr>
    <tr>
      <td style="text-align:left">Arguments<b>: </b>(none)</td>
    </tr>
  </tbody>
</table>
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

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method</b>: <code>tag.NewKey</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Creates or retrieves a string key identified by name. Calling <code>NewKey</code> consequently
        with the same name returns the same key.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>name</code></b> (<em>string</em>): A string by which the tag will
            be referred to. <em>Example: </em><code>&quot;device&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>tag.Insert</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Returns a mutator that inserts a value associated with the key. If the
        key already exists in the tag map, mutator doesn't update the value.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>tag key</code></b> (Key): key which will be set. <em>Example: </em><code>TagKey.create(&quot;device&quot;)</code>
          </li>
          <li><b><code>tag value</code></b> (string<em>): </em>value to set for the given
            key. <em>Example:</em>  <code>TagValue.create(&quot;mobile-ios&quot;)</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>tag.New</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Returns a new context that contains a tag map originated from the incoming
        context and modified with the provided mutators.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments<b>:</b>
        </p>
        <ol>
          <li><b><code>context</code></b> (<em>ctx</em>): The context to modify. <em>Example:</em>  <code>context.Background()</code>
          </li>
          <li><b><code>mutator</code> </b>(<em>Mutator</em>): The mutator to add to
            the returned context. <em>Example: </em><code>tag.insert(KeyDevice, &quot;mobile-ios&quot;)</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
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

Simply create a string and pass it in to `stats.record`. For more information, visit [Recording](recording.md).
{% endtab %}

{% tab title="Python" %}
```python
from opencensus.tags import tag_key as tag_key_module
from opencensus.tags import tag_map as tag_map_module
from opencensus.tags import tag_value as tag_value_module

# Create the tag key
key_device = tag_key_module.TagKey("device")

def main():
  # Create the Tag
  tmap = tag_map_module.TagMap()
  tmap.insert(key_device, tag_value_module.TagValue("mobile-ios"))
  
if __name__ == "__main__":
   main()
```

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>TagKey.TagKey (constructor)</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Create and return a new tag key</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>name</code> (<em>string</em>)<em>:  </em>A string by which the tag
            will be referred to. <em>Example: </em><code>&quot;device&quot;</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>TagMap.TagMap (constructor)</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">A tag map is a map of tags from key to value</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>tags</code> (optional) (<em>tag[]</em>)<em>:  </em>A list of tags
            to append to the tag map <em>Example: </em><code>tag_map_module.TagMap(tags=[{&apos;key1&apos;: &apos;value1&apos;}])</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table><table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Method: </b><code>TagMap.insert</code>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Inserts a key and value in the map if the map does not already contain
        the key.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Arguments:</p>
        <ol>
          <li><code>key</code> (<em>TagKey</em>)<em>:  </em>key which will be set. <em>Example: </em><code>tag_key_module.TagKey(&quot;device&quot;)</code>
          </li>
          <li><code>value</code> (<em>TagValue</em>)<em>:  </em>value to set for the
            given key. <em>Example: </em><code>tag_value_module.TagValue(&quot;mobile-ios&quot;)</code>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}
{% endtabs %}



