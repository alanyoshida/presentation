---
title: "A breaf introduction to Prometheus"
author: "Alan Yoshida"
format: revealjs
---

![Prometheus](https://www.vectorlogo.zone/logos/prometheusio/prometheusio-ar21.svg)

Prometheus Query Language (PromQL)

---

Prometheus has a time series database

---

## Data model

Format:
```
<metric name>{<label name>=<label value>, ...}
```

eg:
```
api_http_requests_total{method="POST", handler="/messages"}
```

---

## Types of metrics

#### Counter
 A _counter_ is a cumulative metric that represents a single [monotonically increasing counter](https://en.wikipedia.org/wiki/Monotonic_function) whose value can only increase or be reset to zero on restart.

#### Gauge
A gauge is a metric that represents a single numerical value that can arbitrarily go up and down.

---

#### Histogram
A _histogram_ samples observations (usually things like request durations or response sizes) and counts them in configurable buckets. It also provides a sum of all observed values.

#### Summary
Similar to a histogram, a summary samples observations (usually things like request durations and response sizes). While it also provides a total count of observations and a sum of all observed values, it calculates configurable quantiles over a sliding time window.

---

## Expression language data types

| Var type | Description |
| - | - |
| String | a simple string value; currently unused |
| Scalar | a simple numeric floating point value |
| Range vector | a set of time series containing a range of data points over time for each time series |
| Instant Vector | a set of time series containing a single sample for each time series, all sharing the same timestamp |

---

### Range Vector Selectors

-   `ms` - milliseconds
-   `s` - seconds
-   `m` - minutes
-   `h` - hours
-   `d` - days - assuming a day has always 24h
-   `w` - weeks - assuming a week has always 7d
-   `y` - years - assuming a year has always 365d

---

## Label and Range Filter

---

## The Operators

The following binary arithmetic operators exist in Prometheus:

| Operator | Description |
| - | - |
| + | addition |
| - | subtraction |
| * | multiplication |
| / | division |
| % | modulo |
| ^ | power/exponentiation |

---

### Comparison binary operators
The following binary comparison operators exist in Prometheus:

| Comparison | Description |
| - | - |
| == | equal |
| != | not-equal |
| > | greater-than |
| < | less-than |
| >= | greater-or-equal |
| <= | less-or-equal |

---

### Logical/set binary operators
These logical/set binary operators are only defined between instant vectors:

| logical | descriptions |
| - | - |
| and | (intersection) |
| or | (union) |
| unless | (complement) |

---

### Aggregation operators
Prometheus supports the following built-in aggregation operators that can be used to aggregate the elements of a single instant vector, resulting in a new vector of fewer elements with aggregated values:

| Operator | Description |
| - | - |
| sum | calculate sum over dimensions |
| min | select minimum over dimensions |
| max | select maximum over dimensions |
| avg | calculate the average over dimensions |
| group | all values in the resulting vector are 1 |
| stddev | calculate population standard deviation over dimensions |
| stdvar | calculate population standard variance over dimensions |
| count | count number of elements in the vector |
| count_values | count number of elements with the same value |
| bottomk | smallest k elements by sample value |
| topk | largest k elements by sample value |
| quantile | calculate φ-quantile | 0 ≤ φ ≤ 1 | over dimensions |

---

## Example

Kube-state-metrics
https://github.com/kubernetes/kube-state-metrics



