---
published: true
title: "[프로그래머스] 모의고사"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04
---

# [완전탐색] 모의고사

> 난이도 : 1

#### 문제

```
수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.
```

<br>

#### 제한사항

- 시험은 최대 10,000 문제로 구성되어있습니다.
- 문제의 정답은 1, 2, 3, 4, 5 중 하나입니다.
- 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

<br>

#### 입출력 예

| answers     | return  |
| ----------- | ------- |
| [1,2,3,4,5] | [1]     |
| [1,3,2,4,2] | [1,2,3] |

<br>

#### 풀이

`완전탐색`, 수포자 1,2,3 의 찍는 패턴이 반복된다. 처음 naive한 시도에서는 이 패턴을 한번 더 패턴화하려고 했었다.

```js
function condition1(a, i) {
  return i % 6 === a[i] - 1;
}

function condition2(a, i) {
  const random = [1, 3, 4, 5];
  return (
    (i % 2 === 0 && a[i] === 2) ||
    (i % 2 === 1 && a[i % 8] === random[(i % 8) / 2 - 0.5])
  );
}

function condition3(a, i) {
  return (
    ((i % 10 === 0 || i % 10 === 1) && a[i] === 3) ||
    ((i % 10 === 2 || i % 10 === 3) && a[i] === 1) ||
    ((i % 10 === 4 || i % 10 === 5) && a[i] === 2) ||
    ((i % 10 === 6 || i % 10 === 7) && a[i] === 4) ||
    ((i % 10 === 8 || i % 10 === 9) && a[i] === 5)
  );
}

for (let i = 0; i < answers.length; i++) {
  if (condition1(answers, i)) {
    score1++;
  }
  if (condition2(answers, i)) {
    score2++;
  }
  if (condition3(answers, i)) {
    score3++;
  }
}

// (...)
```

- 매우 지저분하다. 삽질의 교과서가 있다면 1장에 실릴 법한 사용
- 정답지 `answers` 배열은 무작위이면서 전부 확인해야 하므로(`완전탐색`), 최소 시간복잡도는 `O(N)`이다.
- 애초에 수포자 수는 3명으로 정해져 있고, 반복되는 패턴은 길지 않다 (셋 모두 길이 10 이내).
- 반면 정답지의 길이는 최대 10,000
- 따라서 패턴은 배열에 담아 constant time 으로 접근할 수 있게 만들고,
- 패턴에 맞춰 boolean만 가져오면 되니까, 정답지 배열에 `for` 문 돌리는 대신, `filter` 내장함수 사용한다.

```js
function solution(answers) {
  let score = [];

  const pattern = [
    [1, 2, 3, 4, 5],
    [2, 1, 2, 3, 2, 4, 2, 5],
    [3, 3, 1, 1, 2, 2, 4, 4, 5, 5],
  ];

  score[0] = answers.filter(
    (a, i) => a === pattern[0][i % pattern[0].length]
  ).length;
  score[1] = answers.filter(
    (a, i) => a === pattern[1][i % pattern[1].length]
  ).length;
  score[2] = answers.filter(
    (a, i) => a === pattern[2][i % pattern[2].length]
  ).length;

  let maxScore = Math.max(...score);
  let result = [];

  for (let i = 0; i < score.length; i++) {
    if (score[i] === maxScore) {
      result.push(i + 1);
    }
  }

  return result;
}
```

<br>
<br>
<br>

[맨 위로](#)
