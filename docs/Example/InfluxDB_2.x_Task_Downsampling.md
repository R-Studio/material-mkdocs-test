![../assets/images/in_markdown/00-influxdb_downsampling.png](00-influxdb_downsampling.png "00-influxdb_downsampling.png")

The following downsampling examples calculates **every hour the mean value (for an two hour window) of every series and writes in a new Influx Database** respectively Influx bucket.

## Flux (Tasks) - InfluxDB 2.x

*Flux is InfluxData’s functional data scripting language designed for querying, analyzing, and acting on data. Beginning with InfluxDB 1.8.0, Flux is available for production use along side InfluxQL.*

    option task = {name: "<TASK_NAME>", every: 1h}

    data = from(bucket: "<SOURCE_BUCKET>")
             |> range(start: -duration(v: int(v: task.every) * 2))
             |> filter(fn: (r) =>
                     (r._measurement =~ /.*/))

    data
             |> aggregateWindow(fn: mean, every: 1h)
             |> filter(fn: (r) =>
                     (exists r._value))
             |> to(bucket: "<DESTINATION_BUCKET>", org: "<YOUR_INFLUX_ORGANISATION>")

''More information's about "Flux Tasks": <https://docs.influxdata.com/influxdb/v2.0/process-data/common-tasks/downsample-data/#example-downsampling-task-script>

## InfluxQL (Continuous Queries) - InfluxDB 1.x

*InfluxQL is an SQL-like query language for interacting with InfluxDB. It has been crafted to feel familiar to those coming from other SQL or SQL-like environments while also providing features specific to storing and analyzing time series data. However InfluxQL is not SQL and lacks support for more advanced operations like UNION, JOIN and HAVING that SQL power-users are accustomed to. This functionality is available with Flux.*

    CREATE CONTINUOUS QUERY <CONTINUOUS_QUERY_NAME> ON <INFLUXDB_NAME>
    BEGIN
      SELECT mean(*)
      INTO <INFLUXDB_NAME>.<DESTINATION_RETENTION_POLICY_NAME>.:MEASUREMENT
      FROM <SOURCE_INFLUXDB_NAME>.<SOURCE_RETENTION_POLICY_NAME>./.*/
      GROUP BY time(1h), *
    END

*More information's about "Continuous Queries": <https://docs.influxdata.com/influxdb/v1.8/query_language/continuous_queries/>*

## Example calculation of total Metric-points per bucket

For example we are writting **200 Metric-points per second**, this means:

  - \~ **12'000** Metric-points per minute
  - \~ **720'000** Metric-points per hour
  - \~ **17'000'000** Metric-points per day
  - \~ **518'000'000** Metric-points per month

Assumption we have following Buckets (Influx databases):
![01-influxdb_downsampling.png](01-influxdb_downsampling.png "01-influxdb_downsampling.png")
*red = number of Metric-points, green = number of series, blue = total Metric-points*

# Downsampling Errors

## Unsupported aggregate column type string

### Cause

Your downsampling tasks are failing with the error message **Unsupported aggregate column type string**. This is because you have some **_value with strings in it**. InfluxDB cannot aggregate on values with strings or on datatype string.

### Workaround

  - Find the measurements with strings in "_value" with following search query:

<!-- end list -->

    from(bucket: "<YOUR_BUCKET>")
      |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
      |> filter(fn: (r) => r._value =~ /\D/)
      |> group(columns: ["_measurement"])

  - Filter out these measurements in your InfluxDB task by adding following filter statement:

<!-- end list -->

    ...
      |> filter(fn: (r) => r._measurement != "<MEASUREMENTS_WITH_STRINGS>")
    ...

### Solution

*Unfortunately until now InfluxDB 2.x offers no solution for this.*


