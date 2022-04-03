---
published: true
title: "[프로그래머스] N으로 표현"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-03
last_modified_at: 2022-04-03
---

# [DP] N으로 표현

> 난이도 : 3

#### 문제

```
아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)
12 = 55 / 5 + 5 / 5
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.
```

<br>

#### 제한사항

- `N`은 1 이상 9 이하입니다.
- `number`는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

<br>

#### 입출력 예

| N   | number | return |
| --- | ------ | ------ |
| 5   | 12     | 4      |
| 2   | 11     | 3      |

<br>

#### 풀이

`DP`, 직관성을 위해 길이 9인 배열 `rep`에 저장. (rep[i] ~ rep[8]까지. 예를 들어, N을 5번 사용해서 만들 수 있는 숫자는 rep[5]에 저장)

```js
function solution(N, number) {
  const rep = new Array(9).fill(0).map((el) => new Set());
  for (let i = 1; i < 9; i++) {
    rep[i].add("1".repeat(i) * N);
    for (let j = 1; j < i; j++) {
      for (const item1 of rep[j]) {
        for (const item2 of rep[i - j]) {
          rep[i].add(item1 + item2);
          rep[i].add(item1 - item2);
          rep[i].add(item1 * item2);
          rep[i].add(item1 / item2);
        }
      }
    }
    if (rep[i].has(number)) return i;
  }
  return -1;
}
```

<br>
<br>
<br>

[맨 위로](#)
