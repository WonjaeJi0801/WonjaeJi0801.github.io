---
published: true
title: "[백준] 평범한 배낭"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-05
last_modified_at: 2022-04-05
---

# [DP] 평범한 배낭

#### 문제

```
준서는 여행을 배낭을 최대한 가치 있게 싸려고 한다.
준서가 여행에 필요하다고 생각하는 N개의 물건이 있다. 각 물건은 무게 W와 가치 V를 가지는데, 해당 물건을 배낭에 넣어서 가면 준서가 V만큼 즐길 수 있다. 준서는 최대 K만큼의 무게만을 넣을 수 있는 배낭만 들고 다닐 수 있다. 준서가 최대한 즐거운 여행을 하기 위해 배낭에 넣을 수 있는 물건들의 가치의 최댓값을 알려주자.
```

<br>

#### 입출력 예

출력 :

```
4 7
6 13
4 8
3 6
5 12
```

<br>

출력 :

```
14
```

<br>

#### 풀이

`DP` 문제, `bag[n][k]` = n번까지의 물건들 중 최적으로 고른 물건들(무게 합이 k 이하이면서 가치 합이 최대)의 가치 합

- n번 물건의 무게가 배낭의 무게 한도보다 무거워 넣을 수 없을 때:
  이전 값(n번째 물건 넣기 전의 최적 가치 합)
  `bag[n][k] = bag[n-1][k]`
  <br>
- 넣을 수 있을 때: n번째 물건의 무게 `w`, 가치 `v`
  (이전 값, w만큼 담지못한 이전 값에 n번째 물건 담았을 때 가치 합) 중 큰 것
  `bag[n][k] = Math.max(bag[n-1][k], bag[n-1][k-w] + v)`

```js
var fs = require("fs");

var input = fs
  .readFileSync("/dev/stdin")
  .toString()
  .trim()
  .split("\n")
  .map((el) => el.split(" "));

const [N, K] = input.shift().map((el) => Number(el));
const list = input.map((a) => a.map((e) => Number(e))).filter((c) => c[0] <= K);
list.unshift(undefined);

let bag = new Array(N + 1).fill(0).map((el) => new Array(K + 1).fill(0));

for (let i = 1; i <= N; i++) {
  let [w, v] = list[i];

  for (let j = 0; j <= K; j++) {
    if (j < w) {
      bag[i][j] = bag[i - 1][j];
    } else {
      bag[i][j] = Math.max(bag[i - 1][j], bag[i - 1][j - w] + v);
    }
  }
}

console.log(bag[N][K]);
```

<br>
<br>
<br>

[맨 위로](#)
