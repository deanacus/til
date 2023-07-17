## Syntax

Set some values on records that match `query`

```js
db.<collection>.updateMany(query, {$set: {...changes}})
```

Remove some values from array fields on records that match `query`

```js
db.<collection>.updateMany(query, {$pull: {...changes}})
```