---
published: true
title: "이진 탐색"

categories:
  # pick one
  - Algorithm
tags:
  - [TIL]

toc: true
toc_sticky: true

date: 2022-04-02
last_modified_at: 2022-04-02
---

# 이진 탐색

#### 문제

```
오름차순 정렬된 정수의 배열(arr)과 정수(target)를 입력받아 target의 인덱스를 리턴해야 합니다.
```

##### 입력 1: arr

- `Number` 타입을 요소로 갖는 배열
- arr[i]는 정수
  <br>

##### 입력 2: type

- `Number` 타입의 정수
  <br>

##### 출력

- `Number` 타입을 리턴해야 합니다.
  <br>

##### 주의사항

- **이진탐색 알고리즘**`(O(logN))`을 사용해야 합니다.
- 단순한 배열 순회`(O(N))`로는 통과할 수 없는 테스트 케이스가 존재합니다.
- `target`이 없는 경우, `-1`을 리턴해야 합니다.

<br>

##### 입출력 예시

```js
let output = binarySearch([0, 1, 2, 3, 4, 5, 6], 2);
console.log(output); // --> 2

output = binarySearch([4, 5, 6, 9], 100);
console.log(output); // --> -1
```

<br>

#### 풀이

```js
const binarySearch = function (arr, target) {
  let start = 0;
  let end = arr.length - 1;

  while (start <= end) {
    let middle = Math.floor((start + end) / 2);
    if (arr[middle] < target) {
      start = middle + 1;
    } else if (arr[middle] > target) {
      end = middle - 1;
    } else {
      return middle;
    }
  }

  return -1;
};
```

- arr 의 첫 인덱스와 마지막 인덱스 각각 start, end 할당
- start 와 end 중간값 middle 과 target 대소비교 후, 다음 탐색 범위 재설정 (이전 탐색 범위의 절반: **`O(logN)`**)
- 탐색범위 길이가 1이 될 때까지 반복
- 탐색 반복 후 없으면 -1 리턴

<br/>
<br/>
<br/>

[맨 위로](#)
