---
layout: post
title: "자바스크립트 Array 함수 정리"
date: 2021-03-17 19:44:13 +0900
lastmod: 2021-11-16 18:16:41 +0900
categories: [Javascript]
sitemap:
  changefreq: daily
  priority: 1.0
---

## Array.prototype.sort()

---

> ### sort()는 호출한 배열을 변형함.

### 반환값

**원 배열**이 정렬되어 반환되는 것에 유의.

### 매개변수

compareFunction [Optional]

compareFunction이 제공되지 않으면 **요소를 문자열로 변환**한 후 유니코드 순서에 따라 문자열을 비교하여 정렬된다. 숫자는 9가 80보다 앞에 오지만, 숫자는 문자열로 변환되기 때문에 "80"이 "9"보다 앞에 온다.

compareFunction이 제공되면 compareFunction의 반환 값에 따라 정렬된다.

- 두 엘리먼트 a, b를 전달 받아 반환한 값이
  - **0보다 작을 경우 a가 b보다 앞**에 오도록 정렬. (a, b)
  - **0보다 클 경우 a가 b 뒤**에 오도록 정렬. (b, a)
  - **0을 반환**하는 경우 a와 b의 순서를 변경하지 않음.

### 숫자 정렬 (오름차순)

```javascript
const arr = [2, 1, 3, 10];
arr.sort(function (a, b) {
  if (a === b) return 0;
  return a < b ? -1 : 1;
});
```

### 문자열 정렬

유니코드는 **대소문자를 구분**하기 때문에, 대소문자 구분 없이 문자열을 정렬하기 위해선, compareFunc 내에서 toUpperCase(), toLowerCase() 등을 이용해 비교, 반환하는 코드를 작성해야 한다.

### 객체 정렬

json 객체 내 특정 요소의 값을 기준으로 정렬이 가능하다.

```javascript
const arr = [
  { name: "banana", price: 3000 },
  { name: "apple", price: 1000 },
  { name: "orange", price: 200 },
];
arr.sort(function (a, b) {
  if (a.price === b.price) return 0;
  return a.price > b.price ? 1 : -1;
});
```

<br/>

## Array.prototype.every()

---

every() 메서드는 배열 안의 모든 요소가 주어진 판별 함수를 통과하는지 테스트한다.

> ### every()는 호출한 배열을 변형하지 않음.

### 빈 배열에서 호출하면 무조건 true를 반환.

### 매개변수

1. callback: 각 요소를 테스트할 함수. 아래 세 가지 파라미터를 받는다.
   1. currentValue: 처리할 현재 요소
   2. index[Optional]: 처리할 현재 요소의 인덱스
   3. array[Optional]: every를 호출한 배열
2. thisArg[Optional]: callback을 실행할 때 this로 사용하는 값

### 반환값

callback이 모든 배열 요소에 대해 참인 값을 반환하는 경우 **true**, 그 외엔 **false**

### 동작

callback이 false를 반환하는 요소를 찾을 때 까지 배열에 있는 각 요소에 대해 한 번씩 callback을 실행한다. 해당하는 요소를 발견하는 즉시 every는 탐색을 중단하고 **false**를 반환한다. 그렇지 않고 모든 값에서 true값을 반환하면 **true**를 반환한다.

### 배열 내 모든 요소가 10보다 크거나 같은지 테스트하는 예제

```javascript
const arr = [12, 4, 5, 130, 44];
arr.every((el) => el >= 10); // return false
```

<br/>

## Array.prototype.some()

---

some() 메서드는 배열 안의 어떤 요소라도 주어진 판별 함수를 통과하는지 테스트한다.

> ### some()은 호출한 배열을 변형하지 않음.

### 빈 배열에서 호출하면 무조건 false를 반환

### 매개변수

1. callback: 각 요소를 테스트할 함수. 아래 세 가지 파라미터를 받는다.
   1. currentValue: 처리할 현재 요소
   2. index[Optional]: 처리할 현재 요소의 인덱스
   3. array[Optional]: some를 호출한 배열
2. thisArg[Optional]: callback을 실행할 때 this로 사용하는 값

### 반환값

callback이 어떤 배열 요소라도 참인 값을 반환하는 경우 **true**, 그 외엔 **false** (every와 반대로 동작)

### 동작

callback이 true를 반환하는 요소를 찾을 때 까지 배열에 있는 각 요소에 대해 한 번씩 callback을 실행한다. 해당하는 요소를 발견하는 즉시 some은 탐색을 중단하고 **true**를 반환한다. 그렇지 않고 모든 값에서 true값을 반환하면 **false**를 반환한다.

### 배열 내 10 이상의 값이 있는지 테스트

```javascript
const arr = [12, 4, 5, 130, 44];
arr.some((el) => el >= 10); // return true
```

### 특정 값이 배열에 존재하는지 테스트

```javascript
const arr = [12, 4, 5, 130, 44];
arr.some((el) => el === 34); // return false
arr.some((el) => el === 130); // return true
```

<br/>

## Array.prototype.find()

---

find() 메서드는 주어진 판별 함수를 만족하는 **첫 번째 요소의 값**을 반환. 그런 요소가 없다면 **undefined**를 반환.

> ### find는 호출의 대상이 된 배열을 변형(mutate)하지 않음.

### 매개변수

1. callback: 각 요소를 테스트할 함수. 아래 세 가지 파라미터를 받는다.
   1. currentValue: 처리할 현재 요소
   2. index[Optional]: 처리할 현재 요소의 인덱스
   3. array[Optional]: find를 호출한 배열
2. thisArg[Optional]: callback을 실행할 때 this로 사용하는 값

### 반환값

주어진 판별 함수를 만족하는 첫 번째 요소의 값. 그 외에는 undefined.

### 동작

find 메서드는 callback 함수가 참을 반환 할 때까지 해당 배열의 각 요소에 대해서 callback 함수를 실행한다. 만약 어느 요소를 찾았다면 find 메서드는 해당 요소의 값을 즉시 반환하고, 그렇지 않았다면 undefined를 반환한다. callback은 0 부터 length - 1 까지 배열의 모든 인덱스에 대해 호출되며, 값이 지정되지 않은 요소도 포함하여 모든 인덱스에 대해 호출된다.

#### **find는 탐색 중 배열 내 값이 제거되어도 제거된 값을 탐색한다.**

find 의 callback 함수 구동 중 find를 호출한 배열을 변경하더라도, 최초 find를 호출한 시점의 배열 값을 기준으로 동작을 수행함. 마찬가지로, find가 호출된 뒤에 해당 배열에 값을 추가하더라도 추가된 값을 탐색하지 않음. (아마 find가 호출될 때 호출한 배열을 복사하여 가져온 뒤 동작을 하는 듯.)

### 속성 중 하나를 사용해 배열에서 객체 찾기

```javascript
var arr = [
  { name: "apple", price: 1000 },
  { name: "banana", price: 1000 },
  { name: "cherry", price: 1000 },
];
arr.find((el) => el.name === "banana");
```

<br/>

## Array.prototype.findIndex()

---

find와 동일한 동작을 하나, 반환 값은 해당 요소의 인덱스 값을 반환함.

> ### findIndex는 호출한 배열을 변경하지 않음.

<br/>

## Array.prototype.includes()

---

includes() 메서드는 배열이 특정 요소를 포함하고 있는지 판별

> ### includes는 호출한 배열을 변경하지 않음.

### 매개변수

1. valueToFind: 탐색할 요소. (문자나 문자열을 비교할 때, includes()는 대소문자를 구분함.)

2. fromIndex[Optional]: 검색을 시작할 위치. 기본값 0.

### 인터넷 익스플로러에서 지원되지 않음.

<br/>

## Array.prototype.filter()

---

filter() 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 **새로운 배열로 반환**한다.

> ### filter는 호출되는 배열을 변화시키지 않음.

### 매개변수

1. callback: 각 요소를 테스트하는 함수. true를 반환하면 요소 유지. false를 반환하면 요소를 버림.
   1. element: 처리할 현재 요소
   1. index[Optional]: 처리할 현재 요소의 인데긋
   1. array[Optional]: filter를 호출한 배열
1. thisArg[Optional]: callback을 실행할 때 this로 사용하는 값.

### 반환값

테스트를 통과한 요소들로 이루어진 **새로운 배열**. 어떤 요소도 테스트를 통과하지 못했으면 **빈 배열**을 반환

### 동작

호출한 배열 내 모든 요소에 대해 callback 함수를 호출하고, callbakc 함수가 true를 반환하는 모든 값을 모아 새로운 배열을 만들어 반환함.

### 10보다 작은 값 걸러내기

```javascript
const arr = [11, 2, 14, 40, 9];
const newArr = arr.filter((el) => el >= 10);
```

### 배열 내 객체의 id 값이 number인 값만 남기기

```javascript
const arr = [{ id: 15 }, { id: NaN }, { id: null }];
const newArr = arr.filter(
  (el) => el !== undefined && typeof el.id === "number" && !isNaN(el.id)
);
```

### 배열 내용 검색

```javascript
const fruits = ["apple", "banana", "grapes", "mango", "orange"];
console.log(fruits.filter((el) => el.toLowerCase().indexOf("ap") > -1));
/// 'ap'로 검색하여, 검색 결과는 ['apple', 'grapes']
```

<br/>

## Array.prototype.forEach()

---

주어진 함수를 배열 요소 각각에 대해 실행한다.

> ### forEach는 배열을 변형하지 않으나, callback이 변형할 수는 있음.

### 매개변수

1. callback: 각 요소에 대해 실행할 함수.
   1. currentValue: 처리할 현재 요소
   1. index[Optional]: 처리할 현재 요소의 인덱스
   1. array[Optional]: 호출한 배열
1. thisArg[Optional]: callback을 실행할 때 this로 사용할 값

### 반환값

undefined

### 동작

주어진 callback()을 배열 내 각 요소에 대해 오름차순으로 한 번씩 실행한다. **삭제했거나 초기화하지 않은 인덱스 속성에 대해서는 실행하지 않음.** forEach로 **처리할 요소의 범위는 최초 callback 호출 전에 설정**됨. 호출을 시작한 뒤 배열에 추가한 요소는 callback이 방문하지 않음. 배열의 기존 요소 값이 바뀐 경우, callback에 전달하는 값은 **forEach가 요소를 방문한 시점의 값을 사용**함. 방문하기 **전에 삭제한 요소는 방문하지 않음.**

### forEach는 예외를 던지지 않고는 중간에 멈출 수 없음. 중간에 멈춰야한다면, forEach가 적절한 방법이 아닐지도 모름.

### for 반복문을 forEach()로 바꾸기

```javascript
const items = ["item1", "item2", "item3"];
const copy = [];

for (let i = 0; i < items.length; i++) {
  copy.push(items[i]);
}

items.forEach((item) => {
  copy.push(item);
});
```

<br/>

## Array.prototype.flat()

---

모든 하위 배열 요소를 지정한 깊이까지 재귀적으로 이어붙인 새로운 배열을 생성합니다.

> ### flat은 원 배열을 변형하지 않음.

### 매개변수

### 중첩 배열 평탄화

```javascript
const arr1 = [1, 2, [3, 4]];
arr1.flat();
// [1, 2, 3, 4]

const arr2 = [1, 2, [3, 4, [5, 6]]];
arr2.flat();
// [1, 2, 3, 4, [5, 6]]

const arr3 = [1, 2, [3, 4, [5, 6]]];
arr3.flat(2);
// [1, 2, 3, 4, 5, 6]

const arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

<br/>

## Array.prototype.map()

---

배열 내 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아서 새로운 배열을 반환.

> ### 원 배열을 변형하지 않음. callback 함수에 의해서 변형될 수는 있음.

### 매개변수

1. callback: 각 요소에 대해 실행할 함수.
   1. currentValue: 처리할 현재 요소
   1. index[Optional]: 처리할 현재 요소의 인덱스
   1. array[Optional]: 호출한 배열
1. thisArg[Optional]: callback을 실행할 때 this로 사용할 값

### 반환값

callback의 결과를 모은 새로운 배열

### 동작

callback함수는 (undefined를 포함한) 배열 값이 들어있는 인덱스에 대해서만 호출됨. 즉, 값이 삭제되거나 아직 값이 할당/정의되지 않은 인덱스에 대해서는 호출되지 않음. 명세서에 정의된 알고리즘으로 인해 map을 호출한 배열의 중간이 비어있는 경우, **결과 배열 또한 동일한 인덱스를 빈 값으로 유지**함.

### map이 처리할 요소의 범위는 첫 callback을 호출하기 전에 정해짐.

map이 시작한 이후 배열에 추가되는 요소들은 callback이 호출하지 않음. 배열에 존재하는 요소들의 값이 바뀐 경우, map이 방문하는 시점의 값이 callback에 전달되고, 방문하기 전에 삭제된 요소들은 방문하지 않음.

### map을 활용해 배열 속 객체를 재구성

```javascript
const kvArray = [
  { key: 1, value: 10 },
  { key: 2, value: 20 },
  { key: 3, value: 30 },
];
const reformattedArray = kvArray.map((obj) => {
  let rObj = {};
  rObj[obj.key] = obj.value;
  return rObj;
}); // reformattedArray는 [{1: 10}, {2: 20}, {3: 30}]
```

### 인자를 받는 함수를 사용해 숫자 배열 재구성

```javascript
const numbers = [1, 4, 6];
const doubles = numbers.map((num) => num * 2); // [2, 8, 12]
```

### map을 포괄적으로 사용하기

```javascript
const elems = document.querySelectorAll("select option:checked");
const values = [].map.call(elems, function (obj) {
  return obj.value;
});
```

### 문자열로 작성된 숫자를 number 형식으로 변환

```javascript
["1", "2", "3"].map((str) => parseInt(str));
```

<br/>

## Array.prototype.reduce()

---

배열의 각 욧에 주어진 리듀서 함수를 실행하고, 하나의 결과값을 반환

### 매개변수

1. callback: 배열의 각 요소에 대해 실행할 함수
   1. accumulator: 누산기
   1. currentValue: 현재값
   1. currentIndex: 현재 인덱스
   1. array: 원본 배열
1. initialValue: callback 최초 호출에서 첫번째 인수(acc)에 제공하는 값. 제공하지 않으면 배열의 첫번째 요소를 사용함. 빈 베열에서 초기값 없이 reduce()를 호출하면 오류가 발생.

리듀서 함수의 반환 값은 누산기에 할당되고, 누산기는 순회 중 유지되므로 결국 최종 결과는 하나값이 됨.

### 배열의 모든 값 합산

```javascript
const sum = [1, 2, 3, 4, 5].reduce((acc, cur) => acc + cur, 0);
```

### 객체 배열에서의 값 합산

객체로 이루어진 배열에 들어있는 값을 합산하기 위해서는 초기값을 주어 모든 항목이 **반드시** 작성한 함수를 거치도록 해야함.

```javascript
const sum = [{ x: 1 }, { x: 2 }, { x: 3 }].reduce((acc, cur) => acc + cur.x, 0);
```

### 속성으로 객체 분류하기

```javascript
const people = [
  { name: "Ace", age: 10 },
  { name: "Roopy", age: 20 },
  { name: "Zoro", age: 20 },
];

function groupBy(objArr, props) {
  return objArr.reduce((acc, cur) => {
    let key = cur[props];
    if (!acc[key]) {
      acc[key] = [];
    }
    acc[key].push(cur);
    return acc;
  }, {});
}
const groupedPeople = groupBy(people, "age");
```

### 객체로 이루어진 배열에 담긴 배열 연결하기

```javascript
const friends = [
  {
    name: "Anne",
    books: ["Bible", "Harry Potter"],
    age: 21,
  },
  {
    name: "Bob",
    books: ["War and peace", "Romeo an Juliet"],
    age: 26,
  },
  {
    name: "Alice",
    books: ["The Load of the Rings", "The Shining"],
    age: 18,
  },
];

const allbooks = friends.reduce(
  (acc, cur) => {
    return [...acc, ...cur.books];
  },
  ["Alphabet"]
);
```

### 배열의 중복 항목 제거

```javascript
let arr = [1, 2, 1, 1, 3, 4, 4, 3, 2, 1];
let result = arr.sort().reduce((acc, cur) => {
  const length = acc.length;
  if (length === 0 || acc[length - 1] !== cur) {
    acc.push(cur);
  }
  return acc;
}, []);
```

### 함수 구성을 위한 파이프 함수

```javascript
const double = (x) => x + x;
const triple = (x) => 3 * x;
const quadruple = (x) => 4 * x;

const pipe = (...functions) => (input) =>
  functions.reduce((acc, fn) => fn(acc), input);

const mul6 = pipe(double, triple);
const mul9 = pipe(triple, triple);
const mul16 = pipe(quadruple, quadruple);

const sixsix = mul6(6);
const ninesix = mul9(6);
const sixtsix = mul16(6);
```
