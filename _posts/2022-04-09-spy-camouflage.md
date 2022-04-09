---
published: true
title: "[프로그래머스] 위장"

categories:
  - Programmers
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-09
last_modified_at: 2022-04-09
---

# [해시] 위장

> 난이도 : 2

#### 문제

스파이들은 매일 다른 옷을 조합하여 입어 자신을 위장합니다.

예를 들어 스파이가 가진 옷이 아래와 같고 오늘 스파이가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야 합니다.

| 종류 | 이름                       |
| ---- | -------------------------- |
| 얼굴 | 동그란 안경, 검정 선글라스 |
| 상의 | 파란색 티셔츠              |
| 하의 | 청바지                     |
| 겉옷 | 긴 코트                    |

스파이가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

<br>

#### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 스파이가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '\_' 로만 이루어져 있습니다.
- 스파이는 하루에 최소 한 개의 의상은 입습니다.

<br>

#### 입출력 예

| clothes                                                                                    | return |
| ------------------------------------------------------------------------------------------ | ------ |
| [ ["yellowhat", "headgear"], ["bluesunglasses", "eyewear"], ["green_turban", "headgear"] ] | 5      |
| [ ["crowmask", "face"], ["bluesunglasses", "face"], ["smoky_makeup", "face"] ]             | 3      |

<br>

#### 풀이

`해시`, 객체 C에 종류별 옷 key-value 쌍 저장 후 .map 으로 길이 반환 (clothes 가 모두 distinct 한 값이므로 객체 C에 저장 시 count++ 해도 무관).

C 가 {a: [...(x개)], b: [...(y개)], c: [...(z개)],} 일 때, `null` 을 포함한 옷 조합 수는 (x+1)(y+1)(z+1) - 1 (모든 sort 에서 `null` 선택한 경우).

```js
function solution(clothes) {
  let C = {};

  clothes.forEach((el) => {
    let [wear, sort] = el;
    if (!C[sort]) {
      C[sort] = [wear];
    } else {
      C[sort].push(wear);
    }
  });

  C = Object.values(C).map((el) => el.length);

  return C.reduce((acc, cur) => acc * (cur + 1), 1) - 1;
}
```

<br>
<br>
<br>

[맨 위로](#)
