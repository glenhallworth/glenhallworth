Title: Natural Sort Order In Javascript
Published: 05/01/2019
Tags: ["Javascript", "Sort", "Order"]

---

[Natural sort order](https://blog.codinghorror.com/sorting-for-humans-natural-sort-order/) is the order humans want data in. Standard string sorting is often a poor user experience.

Take an array.

```javascript
["2", "100", "10", "20", "20 SC", "1"];
```

Standard string sorting will result in this order.

```javascript
["1", "10", "100", "2", "20", "20 SC"];
```

We don't want "100" to come before "2".

Thankfully it's easy to sort arrays in Javascript with natural sort order using [localeCompare](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare).

```javascript
const arr = ["2", "100", "10", "20", "20 SC", "1"];
arr.sort((a, b) => {
  return a.localeCompare(b, undefined, { numeric: true });
});
```

This results in an array in this order.

```javascript
["1", "2", "10", "20", "20 SC", "100"];
```
