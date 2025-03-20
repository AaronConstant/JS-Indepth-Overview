# Arrays in JavaScript

## Introduction

In JavaScript, **arrays** are ordered collections of values that can store multiple elements in a single variable. Unlike primitive data types, arrays are **reference types**, meaning they are stored in memory as references rather than direct values. Arrays are flexible, allowing elements of different types and providing various built-in methods for manipulation.

## Creating Arrays

JavaScript provides multiple ways to create an array:

### Using Array Literals (Recommended)
```javascript
const numbers = [1, 2, 3, 4, 5];
```

### Using the `Array` Constructor
```javascript
const fruits = new Array("Apple", "Banana", "Cherry");
```

### Creating an Empty Array
```javascript
const emptyArray = [];
```

## Accessing Elements

Array elements are accessed using **zero-based indexing**.

```javascript
const colors = ["Red", "Green", "Blue"];
console.log(colors[0]); // "Red"
console.log(colors[1]); // "Green"
```

Attempting to access an out-of-bounds index returns `undefined`.

```javascript
console.log(colors[5]); // undefined
```

## Modifying Arrays

### Adding Elements

#### Using `push()` (Add to End)
```javascript
const numbers = [1, 2, 3];
numbers.push(4);
console.log(numbers); // [1, 2, 3, 4]
```

#### Using `unshift()` (Add to Start)
```javascript
numbers.unshift(0);
console.log(numbers); // [0, 1, 2, 3, 4]
```

### Removing Elements

#### Using `pop()` (Remove from End)
```javascript
numbers.pop();
console.log(numbers); // [0, 1, 2, 3]
```

#### Using `shift()` (Remove from Start)
```javascript
numbers.shift();
console.log(numbers); // [1, 2, 3]
```

## Iterating Over Arrays

### Using `for` Loop
```javascript
const items = ["a", "b", "c"];
for (let i = 0; i < items.length; i++) {
  console.log(items[i]);
}
```

### Using `forEach()`
```javascript
items.forEach(item => console.log(item));
```

### Using `map()` (Creates a New Array)
```javascript
const upperCaseItems = items.map(item => item.toUpperCase());
console.log(upperCaseItems); // ["A", "B", "C"]
```

## Array Methods

### Finding Elements

#### Using `indexOf()`
```javascript
const nums = [10, 20, 30, 40];
console.log(nums.indexOf(20)); // 1
console.log(nums.indexOf(50)); // -1 (not found)
```

#### Using `includes()`
```javascript
console.log(nums.includes(30)); // true
console.log(nums.includes(50)); // false
```

### Filtering and Transforming Arrays

#### Using `filter()`
```javascript
const evenNumbers = nums.filter(num => num % 2 === 0);
console.log(evenNumbers); // [10, 20, 30, 40]
```

#### Using `reduce()`
```javascript
const sum = nums.reduce((acc, num) => acc + num, 0);
console.log(sum); // 100
```

### Sorting Arrays

#### Using `sort()` (Alphanumeric Sort by Default)
```javascript
const letters = ["b", "a", "c"];
letters.sort();
console.log(letters); // ["a", "b", "c"]
```

#### Sorting Numbers (Custom Compare Function Required)
```javascript
const numbers = [10, 2, 5, 1];
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 2, 5, 10]
```

### Reversing Arrays
```javascript
const reversed = letters.reverse();
console.log(reversed); // ["c", "b", "a"]
```

## Array Length and Resizing

The `length` property returns the number of elements in an array and can be modified to **truncate** an array.

```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.length = 3;
console.log(numbers); // [1, 2, 3]
```

## Spread Operator (`...`)

The **spread operator** allows copying or merging arrays.

### Copying an Array
```javascript
const original = [1, 2, 3];
const copy = [...original];
console.log(copy); // [1, 2, 3]
```

### Merging Arrays
```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2];
console.log(merged); // [1, 2, 3, 4]
```

## Multi-Dimensional Arrays

JavaScript supports **nested arrays**, allowing the creation of matrices.

```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];
console.log(matrix[1][2]); // 6
```

## Shallow Copies vs Deep Copies

In JavaScript, complex data types (such as arrays and objects) are passed by reference rather than by value. This means that when you create a new variable using techniques like the spread operator (`...`), the top-level elements are copied, but nested objects or arrays are still referenced.

## Example 1: Shallow Copy with a Nested Array

```javascript
const array1 = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
const copyOfArray = [...array1]; // Creates a shallow copy
copyOfArray[1][0] = 44;
console.log('array1:', array1);
```

**Output:**
```
array1: [ [ 1, 2, 3 ], [ 44, 5, 6 ], [ 7, 8, 9 ] ]
```

Even though we used the spread operator to create `copyOfArray`, modifying a nested element (`copyOfArray[1][0] = 44;`) also affected `array1`. This happens because the inner arrays (`[1, 2, 3]`, `[4, 5, 6]`, etc.) are still referenced rather than being deeply copied.

## Example 2: Another Shallow Copy with a Mixed Array

```javascript
const array2 = [1, 2, 3, 4, [5, 6, 7]];
const copyOfArray2 = [...array2]; // Creates a shallow copy
copyOfArray2[4][0] = 55;
console.log('array2:', array2);
```

**Output:**
```
array2: [ 1, 2, 3, 4, [ 55, 6, 7 ] ]
```

Again, modifying the nested array inside `copyOfArray2` also modifies `array2` because the reference to `[5, 6, 7]` was copied rather than its values.

## How to Create a Deep Copy

To avoid this issue, you need to create a **deep copy**, meaning all nested elements are also cloned instead of being referenced. There are multiple ways to do this:

### 1. Using `structuredClone` (Recommended for modern browsers)
```javascript
const deepCopy1 = structuredClone(array1);
deepCopy1[1][0] = 99;
console.log('array1:', array1); // Remains unchanged
```

### 2. Using `JSON.parse(JSON.stringify(...))` (Limited to JSON-safe values)
```javascript
const deepCopy2 = JSON.parse(JSON.stringify(array2));
deepCopy2[4][0] = 88;
console.log('array2:', array2); // Remains unchanged
```
> ⚠ **Limitation:** This method doesn't work with functions, `undefined`, `BigInt`, or special objects like `Date` and `Map`.

### 3. Using `lodash.cloneDeep()`
```javascript
const _ = require('lodash');
const deepCopy3 = _.cloneDeep(array1);
deepCopy3[1][0] = 77;
console.log('array1:', array1); // Remains unchanged
```
This method works reliably for deeply nested structures.

## Conclusion
- The spread operator (`[...]`) creates a **shallow copy**, which means only the top-level structure is duplicated, while nested objects and arrays remain referenced.
- To create a **deep copy**, use `structuredClone()`, `JSON.parse(JSON.stringify())`, or a utility library like Lodash’s `cloneDeep()`.

Understanding the difference between shallow and deep copies is crucial when working with nested data structures in JavaScript to prevent unintended side effects.

```

## Conclusion

Arrays in JavaScript are powerful data structures with numerous built-in methods for manipulation. Understanding them is crucial for writing efficient and effective JavaScript programs.

