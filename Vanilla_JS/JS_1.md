# JS_1

docs/momentum/ 에 `index.html`, `style.css`, `app.js` 생성

### Data types, Variables

```jsx
// app.js

const a = 5; // 변동 가능성이 없을 때 const
const b = 2;
let myName = "jaeyoung"; // 변동 가능성이 있을 때 let

console.log(a+b);
console.log(a*b);
console.log(a/b);
console.log("hello " + myName)

myName = "leejaeyoung";
console.log("your new name is " + myName);
```

```jsx
const amIFat = null; // Boolean형은 "true"가 아니라 true로 입력 (string이 아님)
console.log(amIFat);

let something;
console.log(something); // undefined
```

### Arrays

```jsx
const daysOfWeek = ["mon", "tue", "wed", "thu", "fri", "sat"];

console.log(daysOfWeek); // ['mon', 'tue', 'wed', 'thu', 'fri', 'sat']

// Get Item from Array
console.log(daysOfWeek[4]); // fri

// Add one more day to the Array
daysOfWeek.push("sun")

console.log(daysOfWeek); // ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun']
```

배열은 다양한 자료형이 들어갈 수 있다. 

```jsx
const nonsense = [1, 2, "hello", false, null, true, undefined, "jaeyoung"]
```

### ****Objects****

```jsx
const player = {
    name: "jaeyoung",
    points: 150,
    fat: true,
};

console.log(player);
console.log(player.name);
console.log(player["name"]);

// Add property
player.lastName = "potato";

// Update property
player.points = 15;
```

### ****Functions****

```jsx
function sayHello(nameOfPerson, age){
    console.log("Hello! My name is " + nameOfPerson + " and I'm " + age);
}

sayHello("sinosuke", 5);
sayHello("himawari", 1);
sayHello("siro", 2);
```

```jsx
function plus(a, b) {
    console.log(a + b);
}

function divide(a, b) {
    console.log(a/b);
}

plus(8, 60);
divide(98, 20);
```

Object에 함수를 사용하는 방법

```jsx
const player = {
    name: "jaeyoung",
    sayHello: function(otherPersonsName) {
        console.log("Hello " + otherPersonsName + " nice to meet you!");
    },
}

player.sayHello("siro"); // Hello siro nice to meet you!
```

### 실습

계산기 Object 만들기

```jsx
const calculator = {
    add: function(a, b) {
        console.log(a+b);
    },
    minus: function(a, b) {
        console.log(a-b);
    },
    divide: function(a, b) {
        console.log(a/b);
    },
    multiple: function(a, b) {
        console.log(a*b);
    },
    power: function(a, b) {
        console.log(a**b);
    }
};

calculator.add(5, 2); // 7
calculator.minus(5, 2); // 3
calculator.divide(5, 2); // 2.5
calculator.multiple(5, 2); // 10
calculator.power(5, 2); // 25
```
