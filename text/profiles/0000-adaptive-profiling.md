# Adaptive Profiling: Automatic ON/OFF Based on avg + 3 stddev

Automatically enable or disable profiling in OpenTelemetry based on span duration deviations (ex. exceeding the average plus three standard deviations), reducing the total number of traces and improving resource efficiency.

## Motivation

The goal is to enhance observability by automatically enabling profiling during significant deviations in span duration. This adaptive mechanism helps in quickly identifying and addressing performance issues while reducing resource overhead when the system is performing well.

## Explanation

The proposed adaptive profiling mechanism dynamically turns profiling on or off based on span duration deviations exceeding the average plus three standard deviations.

### How it works:

1. **Data Collection**: Continuously gather span durations.
2. **Threshold Calculation**: Compute the average and standard deviation of these span durations.
3. **Deviation Detection**: Compare the current span durations against the threshold:
   `Threshold = Average + 3 * Standard Deviation`
4. **Profiling Activation**: Enable profiling if any span duration exceeds the threshold.
5. **Profiling Deactivation**: Disable profiling when span durations return to within the normal range.

#### Example:
If the average span duration is 100ms with a standard deviation of 20ms, profiling will be enabled if any span duration exceeds 160ms.

## Internal details

### Impact and Interaction:
- **Existing Functionality**: Profiling is only activated during significant deviations, reducing the total number of traces and thus the resource overhead.
- **Error Modes**: Potential false positives/negatives if the threshold calculation is not accurately tuned.
- **Corner Cases**: Metrics with high variability might frequently trigger profiling; configurable thresholds can mitigate this.

### Implementation Suggestion:
1. **Metric Selection**: Identify key span durations relevant to performance.
2. **Historical Data Storage**: Maintain a rolling window of span duration data for accurate threshold calculations.
3. **Profiling Agents**: Develop agents that can be dynamically enabled/disabled.
4. **Configuration Options**: Allow customization of span durations and thresholds.

## Trade-offs and mitigations

### Drawbacks:
- **Configuration Complexity**: Users may need to fine-tune thresholds for their specific use cases.

### Mitigations:
- **Efficient Implementation**: Optimize the profiling agents for minimal overhead.
- **User Education**: Provide clear documentation and guidelines for configuring thresholds.

## Prior art and alternatives

### Prior Approaches:
- **Manual Profiling**: Users manually enable/disable profiling based on perceived need.
- **Static Thresholds**: Fixed thresholds for enabling profiling, not adaptive to changing workloads.

### Rejected Ideas:
- **Constant Profiling**: Always-on profiling, leading to high overhead.
- **Machine Learning Models**: Considered too complex for initial implementation.

## Open questions

- How should we handle multi-metric deviations (e.g., both CPU and memory)?
- What is the optimal window size for historical data?

## Future possibilities

- **Customizable Thresholds**: Allow users to define custom thresholds or additional conditions.
- **Advanced Anomaly Detection**: Integrate machine learning for improved accuracy.
- **Visualization Tools**: Develop dashboards to visualize profiling data and thresholds.