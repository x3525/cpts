# Kibana Query Language

## Free Text

```kql title="Query"
"svc-sql1"
```

## [Query](https://www.elastic.co/docs/explore-analyze/query-filter/languages/kql#_filter_for_documents_that_match_a_value)

```kql title="Query"
event.code: 4625
```

## Query ([Range](https://www.elastic.co/docs/explore-analyze/query-filter/languages/kql#_filter_for_documents_within_a_range))

```kql title="Query"
@timestamp >= "2023-03-03T00:00:00.000Z" AND @timestamp <= "2023-03-06T23:59:59.999Z"
```

## Query ([Boolean](https://www.elastic.co/docs/explore-analyze/query-filter/languages/kql#_combining_multiple_queries))

```kql title="Query"
event.code: 4625 AND winlog.event_data.SubStatus: 0xc0000072
```

## Query ([Wildcard](https://www.elastic.co/docs/explore-analyze/query-filter/languages/kql#_filter_for_documents_using_wildcards))

```kql title="Query"
user.name: admin*
```
