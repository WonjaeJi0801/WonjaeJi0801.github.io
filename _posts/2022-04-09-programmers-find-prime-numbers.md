---
published: true
title: "[프로그래머스] 소수 찾기"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-09
last_modified_at: 2022-04-09
---

# [완전탐색] 소수 찾기

> 난이도 : 2

#### 문제

```
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.
```

<br>

#### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

<br>

#### 입출력 예

| number | return |
| ------ | ------ |
| "17"   | 3      |
| "011"  | 2      |

<br>

#### 풀이

`완전탐색`, n개 조각을 뽑아서 만들 수 있는 조합 중에서(`getComb(arr, n)`), 가능한 순열 찾은 다음(`getPerm(arr)`), 중복수 제거 ([11, 11] -> [11]), 소수 판별 함수 `isPrime` 으로 필터해 배열 길이 리턴

```js
function isPrime(n) {
  if (n <= 1) return false;
  for (i = 2; i <= Math.sqrt(n); i++) {
    if (n % i === 0) return false;
  }
  return true;
}

function getComb(arr, n) {
  if (n === 1) return arr.map((el) => [el]);
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    let rest = getComb(arr.slice(i + 1), n - 1);
    result = result.concat(rest.map((r) => [arr[i]].concat(r)));
  }
  return result;
}

function getPerm(arr) {
  if (arr.length === 1) return [arr];
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    let rest = getPerm(arr.slice(0, i).concat(arr.slice(i + 1)));
    result = result.concat(rest.map((r) => [arr[i]].concat(r)));
  }
  return result;
}

function solution(numbers) {
  numbers = numbers.split("");
  let result = [];

  for (let i = 1; i <= numbers.length; i++) {
    let combinations = getComb(numbers, i);
    let permutations = combinations
      .map((el) => getPerm(el))
      .flat()
      .map((el) => Number(el.join("")));

    let primes = permutations.filter((num) => isPrime(num));

    result = result.concat(primes);
  }

  result = result.filter((n, idx) => result.indexOf(n) === idx);
  return result.length;
}
```

<br>
<br>
<br>

[맨 위로](#)
