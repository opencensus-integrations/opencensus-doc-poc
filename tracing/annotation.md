# Annotation

Annotations are timestamped strings with optional attributes. Annotations are used like log lines, but centric to the span. Annotations contain a “Description” which help tell the story of the event that occurred.

Example annotations include:

* 0.001s Evaluating database failover rules.
* 0.002s Failover replica selected. attributes:`{replica:ab_001 zone:"xy"}`
* 0.006s Response received.
* 0.007s Response requires additional lookups. attributes:`{fanout:4}`

Annotations provide rich details to debug problems in the scope of a span.

## Description

A Description annotation is simply a moment in time associated with a text description.

```java
// 1. Get the current Span
Span span = tracer.getCurrentSpan();

// 2. Add a simple description
span.addAnnotation("starting work");
```

## Attributes

An annotation can have Attributes \(a key/value map\) that can associate the annotation with additional relevant information.

```java
// 3. Add an annotation with attributes
Map<String, AttributeValue> attributes = new HashMap<>();
attributes.put("work_id", AttributeValue.longAttributeValue(1));
attributes.put("work_url", AttributeValue.stringAttributeValue("http://example.com/my/url/1"));

span.addAnnotation("working", attributes);
```



