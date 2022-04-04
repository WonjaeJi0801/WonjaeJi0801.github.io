---
published: true
title: "[프로그래머스] 완주하지 못한 선수"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04
---

# [해시] 완주하지 못한 선수

> 난이도 : 1

#### 문제

```
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.
```

<br>

#### 제한사항

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- `completion`의 길이는 `participant`의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

<br>

#### 입출력 예

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

<br>

#### 풀이

`해시` 문제, 처음 시도한 naive solution에서는 `completion` 배열 앞에서부터 요소를 하나씩 제거하면서, `participant` 배열에서 같은 값 인덱스를 찾아(`indexOf`) 0으로 할당. `while` 문 끝나고 `participant` 배열에서 0이 아닌 것 리턴하는 방식

```js
function solution(participant, completion) {
  while (completion.length > 0) {
    let comp = completion.shift();
    participant[participant.indexOf(comp)] = 0;
  }
  return participant.filter((p) => p !== 0)[0];
}
```

- 정확도 테스트는 모두 pass, 효율성 테스트는 모두 fail.
- "정렬되지 않은" 두 배열의 value-matching 은 `while` 문에서 n번, 안의 `indexOf` 에서 n번, 즉 `O(N^2)`, 끔찍하다.

```js
function solution(participant, completion) {
  participant.sort();
  completion.sort();

  for (let i = 0; i < participant.length; i++) {
    if (completion[i] !== participant[i]) return participant[i];
  }
}
```

- "정렬된" 두 배열의 value-matching 시간복잡도는 `O(NlogN)` (NlogN + NlogN + N)

<br>
<br>
<br>

[맨 위로](#)
