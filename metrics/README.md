# Metrics

Metrics allow you to record and view quantifiable information. The output of your metrics can be displayed as a histogram, sum, count, or view the last recorded value. The output can be filtered by tags which are noted at the time of recording the measure.

To use Metrics, you must know how to use each of the following components:

* [Measures](measures.md): Declare what quantifiable data you will be recording
* [Tags](tags.md): Add a dimension to your measures
* [Recording](recording.md): Make the tagged measures available for the view
* [Aggregation](aggregation.md): Choose how to group your recordings
* [Views](views.md): Organize your measurements by aggregations and tags to be visualized in a backend

{% hint style="success" %}
**Example**: You run a video compression service and want to record the size of all videos requested to be compressed \(**measure**\) and filter the results by device used by the client \(**tag**\). You organize the recorded data into a histogram \(**aggregation**\) and export your **view** to a backend for visualization. 
{% endhint %}

