---
published: true
title: "[DP] 동전 교환"

categories:
  # pick one
  - Algorithm
tags:
  - [TIL]

toc: true
toc_sticky: true

date: 2022-04-01
last_modified_at: 2022-04-01
---

# [DP] 동전 교환

#### 문제

```
동전의 종류 type으로 만들 수 있는 거스름돈 target 경우의 수를 구해야 합니다.
예를 들어 $50 을 거슬러 줄 때 $10, $20, $50 이 있다면 다음과 같이 4 가지 방법으로 $50을 만들 수 있습니다.
- $50 한 장
- $20 두 장, $10 한 장
- $20 한 장, $10 세 장
- $10 다섯 장

target과 type이 매개변수로 주어질 때, type의 요소로 target을 만들 수 있는 경우의 수를 return 해야 합니다.
```

##### 입력 1: target

- `Number` 타입의 100,000 이하의 자연수
  <br>

##### 입력 2: type

- `Number` 타입을 요소로 갖는 100 이하의 자연수를 담은 배열

<br>

#### 풀이

```js
function getNumberOfRemainings(target, type) {
  let remainings = new Array(target + 1).fill(0);
  remainings[0] = 1;

  for (let i = 0; i < type.length; i++) {
    for (let j = 1; j <= target; j++) {
      // remainings의 인덱스가 type[i] 보다 큰 구간만
      // (작은 구간은 type[i]로 만들 수 없는 금액이기 때문에 탐색할 필요가 없다)
      if (type[i] <= j) {
        // 기존 경우의 수에 type[i]를 뺀 금액을 만들 수 있는 경우의 수를 더해준다
        remainings[j] += remainings[j - type[i]];
      }
    }
  }
  return remainings[target];
}
```

- remainings 이라는 배열에 금액을 만들 수 있는 경우의 수를 기록
  - 각 인덱스 no: 만드려는 금액을 의미
- 0 을 만들 수 있는 경우는 아무것도 선택하지 않으면 되기 때문에 remainings[0] = 1 로 초기값 설정

[맨 위로](#)
