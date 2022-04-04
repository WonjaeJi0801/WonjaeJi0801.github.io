---
published: true
title: "코딩테스트에서의 시간복잡도"

categories:
  - Algorithm
tags:
  - [coding test, TIL]

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04
---

### 데이터 크기에 따른 시간복잡도

![big-O-complexity-chart](/assets/images/big-o-complexity-chart.png)

대략적인 기준입니다.

| 데이터 크기 제한 | 예상 시간복잡도     |
| ---------------- | ------------------- |
| n ≤ 1,000,000    | `O(N)` or `O(logN)` |
| n ≤ 10,000       | `O(N^2)`            |
| n ≤ 500          | `O(N^3)`            |

ex. 입력 데이터 크기가 최대 100,000이라면, `O(n^2)`보다는 작은 복잡도 `O(NlogN)` > `O(N)`> `O(logN)`> `O(1)` 를 고려해야 합니다.

<br>

### 자바스크립트 Array 메서드 시간복잡도

[원문](https://dev.to/lukocastillo/time-complexity-big-0-for-javascript-array-methods-and-examples-mlg)

#### Mutator Methods

##### 1. push() - O(1)

##### 2. pop() - O(1)

##### 3. shift() - O(N)

##### 4. unshift() - O(N)

##### 5. splice() - O(N)

##### 6. sort() - O(N logN)

#### Accessor Methods

##### 1. concat() - O(N)

##### 2. slice() - O(N)

##### 3. indexOf() - O(N)

#### Iteration Methods

##### 1. forEach() - O(N)

##### 2. map() - O(N)

##### 3. filter() - O(N)

##### 4. reduce() - O(N)

#### Bonus!!!

##### 1. some() - O(N)

##### 2. every() - O(N)

```js
function solution(exam, ple) {
  return exam + ple;
}
```

<br>
<br>
<br>

[맨 위로](#)
