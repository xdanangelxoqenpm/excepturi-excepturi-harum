## Syntactic sugar wrapper over JS sort function with better readability

Zero-dependency minimal package (two functions) to provide better-readable (albeit more verbose) wrapper over JS default `Array.sort()`. Fully typed.

Prevents errors when interchanging signs in the native `Array.sort()` function like this:

`foo.sort((a, b) => (a.date.isBefore(b.date) ? -1 : 1))` - at the first glance, is this sorted in ASC or DESC order??

Usage:

```javascript
import { sortAsc } from "./src/index";

const array = [3, 1, 4, 1, 5, 9, 2, 6];

const sortedArray = sortAsc({
  array,
  firstIsGreater: (a, b) => a > b, // Compare function that returns boolean
});

console.log(sortedArray); // [1, 1, 2, 3, 4, 5, 6, 9]

console.log(array); // [3, 1, 4, 1, 5, 9, 2, 6] - keeps the original array intact
```

You can provide `areEqual` function parameter to ensure [stability](ttps://en.wikipedia.org/wiki/Sorting_algorithm#Stability) of the sorting function. Otherwise it sorts equal elements in non-deterministic order.

```javascript
import { sortAsc } from "./src/index";

// The sortAsc function is a stable sort when `areEqual` is provided, meaning that it preserves the original input order of elements that are equal.

const inputArray = [
  { name: "John", age: 25 },
  { name: "Alice", age: 30 },
  { name: "Bob", age: 20 },
  { name: "Mike", age: 25 },
];

const sortedArray = sortAsc({
  array: inputArray,
  firstIsGreater: (a, b) => a > b,
  areEqual: (a, b) => a === b,
});

console.log(sortedArray); // [{ name: "Bob", age: 20 }, { name: "John", age: 25 }, { name: "Mike", age: 25 }, { name: "Alice", age: 30 }] - keeps equal elements in order they appear in input
```
