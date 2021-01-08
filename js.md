## The art of avoiding `if-else`:

Example 1: Check if the value is number

```javascript
function isNum (x){
  if (typeof x === 'number')
    return true;
  else
    return false;
}

function betterIsNum (x){
  return typeof x === 'number';
}
```

Example 2: Find a celebrity by name

```javascript
function selector(x) {
  if (x === "Jhon") {
    return `Jhonny Depp`;
  }
  if (x === "Dave") {
    return `Dave Grohl`;
  }
  if (x === "Jim") {
    return `Jim Morrison`;
  }
  if (x === "Katty") {
    return `Katty Perry`;
  }
  if (x === "Tom") {
    return `Tom Hardy`;
  }
  return "Dont have this name >(";
}

function betterSelector(x) {
  const selectorObj = {
    Jhon: "Jhonny Depp",
    Dave: "Dave Grohl",
    Jim: "Jim Morrison",
    Katty: "Katty Perry",
    Tom: "Tom Hardy"
  };
  return selectorObj[x] || "Dont have this name";
}

console.log(betterSelector('Jhon'));
console.log(betterSelector('qwe'));
console.log(selector('Jhon'));
```

Example 3: Filter odd numbers

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let even = numbers.filter(
  (num) => {
    return !Math.abs(num % 2)
  }
);

console.log(even);
```

