# Aggregation

Mongo aggregation is basically a query/transformation pipeline. It is broken up
into a bunch of different stages, allowing you to build very complex and
powerful queries.

- $addFields
- $bucket
- $count
- $documents
- $fill
- $group
- $limit
- $lookup
- $match
- $merge
- $out
- $project
- $replaceRoot
- $set
- $setWindowFields
- $skip
- $sort
- $unionWith
- $unset
- $unwind

## Syntax

```js
db.products.aggregate([...stages]);
```