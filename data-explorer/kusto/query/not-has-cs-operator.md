---
title: The case-sensitive !has_cs string operator - Azure Data Explorer
description: This article describes the case-sensitive !has_cs string operator in Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: alexans
ms.service: data-explorer
ms.topic: reference
ms.date: 09/30/2021
ms.localizationpriority: high
---
# !has_cs operator

Filters a record set for data that does not have a matching case-sensitive string.

The following table provides a comparison of the `has` operators:

|Operator   |Description   |Case-Sensitive  |Example (yields `true`)  |
|-----------|--------------|----------------|-------------------------|
|[`has`](has-operator.md) |Right-hand-side (RHS) is a whole term in left-hand-side (LHS) |No |`"North America" has "america"`|
|[`!has`](not-has-operator.md) |RHS isn't a full term in LHS |No |`"North America" !has "amer"`|
|[`has_cs`](has-cs-operator.md) |RHS is a whole term in LHS |Yes |`"North America" has_cs "America"`|
|[`!has_cs`](not-has-cs-operator.md) |RHS isn't a full term in LHS |Yes |`"North America" !has_cs "amer"`|

> [!NOTE]
> The following abbreviations are used in the table above:
>
> * RHS = right hand side of the expression
> * LHS = left hand side of the expression

For further information about other operators and to determine which operator is most appropriate for your query, see [datatype string operators](datatypes-string-operators.md). 

## Performance tips

> [!NOTE]
> Performance depends on the type of search and the structure of the data.

For faster results, use the case-sensitive version of an operator, for example, `has_cs`, not `has`.

If you're testing for the presence of a symbol or alphanumeric word that is bound by non-alphanumeric characters at the start or end of a field, for faster results use `has` or `in`. 

For best practices, see [Query best practices](best-practices.md).

## Syntax

*T* `|` `where` *col* `!has_cs` `(`*expression*`)`  

## Arguments

* *T* - The tabular input whose records are to be filtered.
* *col* - The column to filter.
* *expression* - Scalar or literal expression.

## Returns

Rows in *T* for which the predicate is `true`.

## Example

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
StormEvents
    | summarize event_count=count() by State
    | where State !has_cs "new"
    | count
```

**Output**

|Count|
|-----|
|67|