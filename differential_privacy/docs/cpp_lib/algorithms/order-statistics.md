

<!-- This file is auto-generated. Do not edit. -->

# Order Statistics

We support multiple algorithms obtaining
[order statistics](https://github.com/google/differential-privacy/blob/master/differential_privacy/algorithms/order-statistics.h). Order
statistics are specific quantiles of an input set. The following algorithms are
supported:

*   `Max`
*   `Min`
*   `Median`
*   `Percentile` for percentile `p`.

Notice that the `Percentile` algorithm can be used to find maximum, minimum, or
median.

## Input & Output

The order statistics algorithms support any numeric type. `Output`s contains an
element with a single value when extracting the result.

## Construction

The order statistics algorithms are
[bounded algorithms](https://github.com/google/differential-privacy/blob/master/differential_privacy/docs/cpp_lib/algorithms/bounded-algorithm.md).
However, when bounds are not manually set, the algorithms do not infer input
bounds by spending privacy budget. Instead, they use the numeric limits of the
input type as bounds. All order statistics algorithms also contain additional
parameters, as we see in the example below. The `Percentile` algorithm requires
the additional parameter `percentile`. For example, when constructing the
`Percentile` algorithm:

```
util::StatusOr<std::unique_ptr<Percentile<T>>> percentile =
   Percentile<T>::Builder.SetAccuracy(T accuracy)
                         
                         .SetPercentile(double percentile)
                         .Build();
```

*   `T`: The input type, for example `double` or `int64`.
*   `T accuracy`: The order statistics algorithms use an underlying noisy binary
    search to find the desired quantile. This parameter corresponds to how many
    rounds of binary search to perform. Each round of the search consumes a
    fraction of the privacy budget.

    
*   `double percentile`: This parameter is required for the `Percentile`
    algorithm and cannot be set for the other order statistics algorithms. It is
    the percentile you wish to find.

## Use

The order statistics algorithms are
[`Algorithm`s](https://github.com/google/differential-privacy/blob/master/differential_privacy/docs/cpp_lib/algorithms/algorithm.md) and supports its full
API.

### Result Performance

For order statistics algorithms, calling `Result` has a time complexity of O(n).
Since all inputs are stored in an internal vector,
space complexity is O(n).
