# Aggregation

Using aggregations, you can view your data as a histogram, sum, count, or view the last recorded value.

{% hint style="success" %}
**Example**: Using a histogram aggregation, you can group all video sizes in these buckets: &gt;=0mb, &gt;=1mb, &gt;=10mb, &gt;=100mb.
{% endhint %}

### Usage

Creating an Aggregation is a crucial prerequisite of creating a [View](views.md).

{% tabs %}
{% tab title="Java" %}
```java
import io.opencensus.stats.Aggregation.Count;
import io.opencensus.stats.Aggregation.Distribution;
import io.opencensus.stats.Aggregation.LastValue;
import io.opencensus.stats.Aggregation.Mean;
import io.opencensus.stats.Aggregation.Sum;
import io.opencensus.stats.BucketBoundaries;

public class App {
    // Count
    private static final AggregationCount =
        Count.create();
    
    // Last Value
    private static final AggregationLastValue =
        LastValue.create();
    
    // Sum
    private static final AggregationSum =
        Sum.create();
    
    // Distribution (Histogram)
    private static final AggregationDistribution =
        Distribution.create(
            BucketBoundaries.create(
                Arrays.asList(
                    0.0, // 0 - 1
                    1.0, // 1 - 10
                    10.0 // 10 - 100
                    100.0 // >= 100
                )
            )
        );
}
```
{% endtab %}

{% tab title="Go" %}
```go
import (
	"go.opencensus.io/stats/view"
)

var (
    // Count
	AggregationCount = view.Count()
	
	// Last Value
	AggregationLastValue = view.LastValue()
	
	// Sum
	AggregationSum = view.Sum()
	
	// Distribution (Histogram)
	AggregationDistribution = view.Distribution(
		0, // 0 - 1
		1, // 1 - 10
		10, // 10 - 100
		100 // >= 100
	)
)
```
{% endtab %}

{% tab title="Node.js" %}
```javascript
import { AggregationType } from "@opencensus/core";

// Count
const aggregationCount = AggregationType.COUNT;

// Last Value
const aggregationLastValue = AggregationType.LAST_VALUE;

// Sum
const aggregationSum = AggregationType.SUM;

// Distribution (Histogram)
const aggregationDistribution = AggregationType.DISTRIBUTION;
const histogramBuckets = [
  0,
  1,
  10,
  100
];
```
{% endtab %}

{% tab title="Python" %}
```python
from opencensus.stats import aggregation as aggregation_module

# Count
COUNT_DISTRIBUTION = aggregation_module.CountAggregation()

# Last Value
LAST_VALUE_DISTRIBUTION = aggregation_module.LastValueAggregation()

# Sum
SUM_DISTRIBUTION = aggregation_module.SumAggregation()

# Distribution (Histogram)
AGGREGATION_DISTRIBUTION = aggregation_module.DistributionAggregation([
  0.0,
  1.0,
  10.0,
  100.0
])
```
{% endtab %}
{% endtabs %}

