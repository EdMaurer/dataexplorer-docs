---
title: Query consistency policy - Azure Data Explorer
description: This article describes the query consistency policy in Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: yonil
ms.service: data-explorer
ms.topic: reference
ms.date: 09/01/2021
---
# Query consistency policy

A workload group's query consistency policy allows specifying options that control the consistency of queries.

## The policy object

Each option consists of:

* A typed `Value` - the value of the limit.
* `IsRelaxable` - a boolean value that defines if the option can be relaxed by the caller, as part of the request's [Client request properties](../api/netfx/request-properties.md).

The following limits are configurable:

| Name                   | Type                 | Description                                                                                      | Supported values                           | Default value | Matching client request property |
|------------------------|----------------------|--------------------------------------------------------------------------------------------------|--------------------------------------------|---------------|----------------------------------|
| QueryConsistency     | `QueryConsistency` | The [consistency](../concepts/queryconsistency.md) to use.                                       | `Strong`, `Weak`, or `WeakAffinitized`     | `Strong`      | `queryconsistency`               |
| CachedResultsMaxAge    | `timespan`           | The maximum age of [cached query results](../query/query-results-cache.md) that can be returned. | A non-negative `timespan`                  | `null`        | `query_results_cache_max_age`    |

> [!NOTE]
> The default value applies in the following cases:
>  * The policy isn't defined, and the client request option isn't set.
>  * The policy is defined, the option isn't defined, and the client request option isn't set.
>  * The policy is defined, the option is defined with `null` as its `Value`, and the client request option isn't set.

### Example

```json
{
  "QueryConsistency": {
    "IsRelaxable": true,
    "Value": "Weak"
  },
  "CachedResultsMaxAge": {
    "IsRelaxable": true,
    "Value": "05:00:00"
  }
}
```

## Control commands

Manage the workload group's query consistency policy with [Workload groups control commands](workload-groups-commands.md).