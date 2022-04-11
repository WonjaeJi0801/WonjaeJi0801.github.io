---
published: true
title: "[Toy] 25_robotPath"

categories:
  - Toy
tags:
  - [coding test, toy]

toc: true
toc_sticky: true

date: 2022-04-10
last_modified_at: 2022-04-10
---

# [BFS] robotPath

#### 문제

세로와 가로의 길이가 각각 `M`, `N` 인 방의 지도가 2차원 배열로 주어졌을 때, 1은 장애물을 의미하고 0 이동이 가능한 통로를 의미합니다. 로봇은 지도 위를 일분에 한 칸씩 상하좌우로 이동할 수 있습니다. 로봇의 위치와 목표 지점이 함께 주어질 경우, 로봇이 목표 지점까지 도달하는 데 걸리는 최소 시간을 리턴해야 합니다.

<br>

#### 제한사항

- `M`, `N` 은 20 이하의 자연수입니다.
- `src`, `dst` 는 항상 로봇이 지나갈 수 있는 통로입니다.
- `src` 에서 `dst` 로 가는 경로가 항상 존재합니다.

<br>

#### 입출력 예

```js
let room = [
  [0, 0, 0, 0, 0, 0],
  [0, 1, 1, 0, 1, 0],
  [0, 1, 0, 0, 0, 0],
  [0, 0, 1, 1, 1, 0],
  [1, 0, 0, 0, 0, 0],
];
let src = [4, 2];
let dst = [2, 2];
let output = robotPath(room, src, dst);
console.log(output); // --> 8
```

<br>

#### 풀이

간단한 `BFS` 구현 문제

```js
const robotPath = function (room, src, dst) {
  const N = room.length;
  const M = room[0].length;
  let loc = [src];
  let time = -1;

  const nextValidLoc = (curArr) => {
    let result = [];
    curArr.forEach(([y, x]) => {
      let output = [
        [y + 1, x],
        [y - 1, x],
        [y, x + 1],
        [y, x - 1],
      ];

      output = output.filter((el) => {
        return (
          el[0] >= 0 &&
          el[0] < N &&
          el[1] >= 0 &&
          el[1] < M &&
          room[el[0]][el[1]] !== "x" &&
          room[el[0]][el[1]] !== 1
        );
      });

      output.forEach((el) => result.push(el));
    });
    return result;
  };

  while (room[dst[0]][dst[1]] === 0) {
    loc.forEach((el) => {
      room[el[0]][el[1]] = "x";
    });
    loc = nextValidLoc(loc);
    time++;
  }

  return time;
};
```

<br>

#### Reference

```js
const robotPath = function (room, src, dst) {
  const aux = (M, N, candi, step) => {
    // 현재 위치
    const [row, col] = candi;

    // 배열의 범위를 벗어난 경우
    if (row < 0 || row >= M || col < 0 || col >= N) return;

    if (room[row][col] === 0 || room[row][col] > step) {
      room[row][col] = step;
    } else {
      // 장애물(1)이거나 이미 최소 시간(1)으로 통과가 가능한 경우
      return;
    }

    // dfs로 4가지 방향에 대해 탐색을 한다.
    // 완전탐색을 해야하므로 bfs나 dfs가 큰 차이가 없다.
    // bfs의 경우 목적지에 도착하는 경우 탐색을 중단해도 되므로,
    // 약간 더 효율적이다.
    aux(M, N, [row + 1, col], step + 1); // 상
    aux(M, N, [row - 1, col], step + 1); // 하
    aux(M, N, [row, col - 1], step + 1); // 좌
    aux(M, N, [row, col + 1], step + 1); // 우
  };

  // 로봇이 서 있는 위치를 1로 초기화하면 (다시 방문하지 않기 위해서),
  // 바로 옆 통로는 2가 된다.
  // 계산이 완료된 후에 최종값에 1을 빼주면 된다.
  aux(room.length, room[0].length, src, 1);

  const [r, c] = dst;
  return room[r][c] - 1;
};
```

<br>
<br>
<br>

[맨 위로](#)
