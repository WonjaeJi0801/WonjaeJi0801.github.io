---
published: true
title: "[프로그래머스] 베스트앨범"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-09
last_modified_at: 2022-04-09
---

# [해시] 베스트앨범

> 난이도 : 3

#### 문제

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

<br>

#### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

<br>

#### 입출력 예

| genres                                          | plays                      | return       |
| ----------------------------------------------- | -------------------------- | ------------ |
| ["classic", "pop", "classic", "classic", "pop"] | [500, 600, 150, 800, 2500] | [4, 1, 3, 0] |

<br>

#### 풀이

`해시`, 객체 rank에 장르 key로, 장르별 재생수 합과 [고유번호, 재생수] 배열 저장. 재생수 합 내림차순으로 장르 정렬 후, 곡 별 재생 수 내림차순 정렬, 길이 2로 잘라서 리턴. 시간복잡도는 `O(NlogN)`

```js
function solution(genres, plays) {
  let rank = {};
  for (let i = 0; i < genres.length; i++) {
    if (!rank[genres[i]]) {
      rank[genres[i]] = [plays[i], [i, plays[i]]];
    } else {
      rank[genres[i]][0] += plays[i];
      rank[genres[i]].push([i, plays[i]]);
    }
  }

  return Object.values(rank)
    .sort((a, b) => b[0] - a[0])
    .map((el) => el.slice(1))
    .map((el) => {
      return el
        .sort((a, b) => b[1] - a[1])
        .map((el) => el[0])
        .slice(0, 2);
    })
    .flat();
}
```

<br>
<br>
<br>

[맨 위로](#)
