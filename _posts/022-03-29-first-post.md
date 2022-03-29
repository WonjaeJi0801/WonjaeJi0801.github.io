---
title: "포스트 제목입니다."
excerpt: "포스팅 미리보기에 표시되는 컨텐츠입니다."

categories:
  - coding test
tags:
  - [toy]

toc: true
toc_sticky: true

date: 2022-03-29
last_modified_at: 2022-03-29
---

# [스택/큐] 문제 제목

> 난이도 : 2

### 문제

```
문제를 설명합니다.

예를 들어 문제를 자세히 설명합니다.

예시와 관련된 데이터가 제시됩니다.

~가 매개변수로 주어질 때
~을 return 하도록 solution 함수를 작성해주세요
```

```
조건
- A는 길이 1 이상 1000 이하인 정수 배열입니다.
- 모든 B는 1 이상 1000 이하입니다.
- C가 없다면 -1을 리턴해야 합니다.
```

<br>

### 풀이

`스택`, `큐`로 분류 되어있는 문제지만 굳이 `스택`으로 풀 필요 없이 `벡터`로 구현하는게 더 나을 것 같아서 벡터로 구현했다. 크기 비교로 원소를 순회할 때 `스택`은 어쩔 수 없이 `pop` 연산을 해야지만 순회할 수 있기 때문이다. `pop`되지 않고 저장 되있는 체로 순회를 해야 다음 원소도 비교할 수가 있기 때문에 `벡터`를 사용했다. 굳이 `스택`을 쓴다면 스택 2 개를 사용해야 할 것 같다.

```cpp
#include <string>
#include <vector>

using namespace std;

vector<int> solution(vector<int> heights) {
    vector<int> answer;

    for(int i = 0; i < heights.size(); i++)
    {
        answer.push_back(0);
    }

    for(int i = heights.size() - 1; i >= 0; i--)
    {
        for(int j = i - 1; j >= 0; j--)
        {
            if (heights[j] > heights[i])
            {
                answer[i] = j + 1;
                break;
            }
        }
    }

    return answer;
}
```

- 예시
  - [6, 9, 5, 7, 4]
- 우선, 답이 될 `answer`벡터에 0 으로 전부 추가해준다. 딱 `heights` 벡터의 크기 만큼.
  - 기본 값으로 0 을 먼저 깔아줌
  - [0, 0, 0, 0, 0]
- i = 4
  - j = 3
    - 7 > 4 이므로 7 의 인덱스(3) + 1 인 4 를 answer[4]에 추가해준다. + break
- i = 3
  - j = 2
  - j = 1
    - 9 > 7 이므로 9 의 인덱스(1) + 1 인 2 를 answer[3]에 추가해준다. + break
- i = 2
  - j = 1
    - 9 > 5 이므로 9 의 인덱스(1) + 1 인 2 를 answer[2]에 추가해준다. + break
- i = 1
  - j = 0
    - answer[1]은 0 으로 남는다.
- i = 0
  - j 가 음수가 되서 두번째 반복문은 안돌게 됨.
    - answer[0]은 0 으로 남는다.

---

```
- key 1

- key 2

- key 3
```

[맨 위로](#)
