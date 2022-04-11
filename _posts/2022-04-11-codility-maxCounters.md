---
published: true
title: "[코딜리티] maxCounters"

categories:
  - Codility
tags:
  - [coding test]

toc: true
toc_sticky: true

date: 2022-04-11
last_modified_at: 2022-04-11
---

# [Counting] maxCounters

#### 문제

You are given N counters, initially set to 0, and you have two possible operations on them:

increase(X) − counter X is increased by 1,
max counter − all counters are set to the maximum value of any counter.
A non-empty array A of M integers is given. This array represents consecutive operations:

if A[K] = X, such that 1 ≤ X ≤ N, then operation K is increase(X),
if A[K] = N + 1 then operation K is max counter.
For example, given integer N = 5 and array A such that:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
the values of the counters after each consecutive operation will be:

    (0, 0, 1, 0, 0)
    (0, 0, 1, 1, 0)
    (0, 0, 1, 2, 0)
    (2, 2, 2, 2, 2)
    (3, 2, 2, 2, 2)
    (3, 2, 2, 3, 2)
    (3, 2, 2, 4, 2)
The goal is to calculate the value of every counter after all operations.

Write a function:

function solution(N, A);

that, given an integer N and a non-empty array A consisting of M integers, returns a sequence of integers representing the values of the counters.

Result array should be returned as an array of integers.

For example, given:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
the function should return [3, 2, 2, 4, 2], as explained above.

Write an efficient algorithm for the following assumptions:

N and M are integers within the range [1..100,000];
each element of array A is an integer within the range [1..N + 1].

<br>

#### 첫번째 풀이

```js
function solution(N, A) {
    let result = new Array(N).fill(0);
    let counter = 0;

    for (let i = 0; i < A.length; i++) {
        if (A[i] <= N) {
            result[A[i]-1]++;
            if (counter < result[A[i]-1]) {
                counter = result[A[i]-1];
            }
        } else {
            result = new Array(N).fill(counter);
        }
    }

    return result;
}
```
correctness 는 모두 통과하지만 performance test case 5개 중 2개를 통과하지 못한다. (77%)
for 문 안에서 `.fill`을 돌리니 `O(N^2)` 이 나온 것.

<br>

#### 두번째 풀이

따라서 가장 큰 카운터를 저장 및 갱신하는 `max` 변수를 만들고,
for 문이 끝난 다음에 `.map` 으로  `max` 값을 할당해주었다.
Detected time complexity: `O(N+M)` 로 나오며 모든 test case 통과 (100%)


```js
function solution(N, A) {
    let result = new Array(N).fill(0);
    let counter = 0;
    let max = 0;

    for (let i = 0; i < A.length; i++) {
        if (A[i] <= N) {
            if (result[A[i]-1] < max) {
                result[A[i]-1] = max;
            }
            result[A[i]-1]++;
            if (counter < result[A[i]-1]) {
                counter = result[A[i]-1];
            }
        } else {
            max = counter;
        }
    }

    return result.map((el) => {
        if (el < max) {
            return max;
        } else {
            return el;
        }
    });
}
```

<br>
<br>
<br>

[맨 위로](#)
