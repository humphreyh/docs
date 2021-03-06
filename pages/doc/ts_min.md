---
title: min Function
keywords: query language reference
tags: [reference page]
sidebar: doc_sidebar
permalink: ts_min.html
summary: Reference to the min() function
---
## Summary
```
min(<expression1>, <expression2>)
min(<expression>[,metrics|sources|sourceTags|tags|<pointTagKey>])
```

When used as a comparison function, returns the lower of the two values in `expression1` and `expression2`.
When used as an aggregation function, returns the lowest value of all series. If there are gaps of data, they will first be filled in with interpolation. Use `rawmin` to return the highest value of all series without interpolation.

## Parameters

### Comparison Function
<table>
<tbody>
<thead>
<tr><th width="20%">Parameter</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td>expression1</td>
<td>Expression to compare with another expression or with a metric, source, sourceTag(s), tag(s), or pointTagKey. </td></tr>
<tr>
<td>expression2</td>
<td>Second expression of two expressions to compare.   </td>
</tr>
</tbody>
</table>

### Aggregation Function
<table>
<tbody>
<thead>
<tr><th width="20%">Property</th><th width="80%">Description</th></tr>
</thead>
<tr>
<td markdown="span"> [expression](query_language_reference.html#expressions)</td>
<td>Expression to find the minimum for. </td></tr>
<tr>
<td>metrics&vert;sources&vert;sourceTags&vert;tags&vert;&lt;pointTagKey&gt;</td>
<td>Optional additional expressions to filter or group the minimum by. </td>
</tr>
</tbody>
</table>

## Description

You can use `min()` as a comparison function or as an aggregation function.

### Comparison Function

The `min()` comparison function lets you display all data points above a desired threshold, and assigns the threshold value to all data points below the threshold.

### Aggregation Function

When you add `min()` to a ts() expression, Wavefront sorts the values at each time interval and displays the highest (maximum) data value across all reporting metrics and sources.

The `min()` aggregation function interpolates the points of the underlying set of series, and then applies the function to the interpolated series. Use `rawmax` to not use interpolation. See [Standard Versus Raw Aggregate Functions](query_language_aggregate_functions.html).

To provide an aggregate value that our customers typically expect, Wavefront attempts to interpolate all queries at a time slice if at least one real reported data value is present.

For a live-view chart, where interpolation is not possible because no new points have been reported yet, Wavefront associates the last known reported value for all queries if a real reported data value is present. We apply the last known reported value only if interpolation can’t occur AND the last known reported point has been reported within the last 15% of the query time in the chart window.

## Examples

### Comparison Function

The following example from our built-in Interactive Query Language Tutorial illustrates the use of `min`. It includes a mar line so you can see how the values are shown as 200 if they are higher than the specified minimum.

![ts min](images/ts_min.png)

### Aggregation Function

The following example shows `min()` without an expression to compare against. For this example, we can group the results.

![ts min aggr](images/ts_min_aggr.png)


## Caveats

Sometimes it's best to use `min` with `align`. See [The align() Function](query_language_align_function.html)
