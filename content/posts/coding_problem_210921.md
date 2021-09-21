---
title: 추석 트래픽의 최대 처리량 구하기 
date: "2021-09-20T22:41:32.169Z"
template: "post"
draft: false
slug: "coding-problem-210920"
category: "Coding Problem"
tags:
  - "Javascript"
  - "Alagorithm"
  - "Web Development"
description: "2018 KAKAO, Lv.3"
socialImage: "/media/coding_problem/chuseok.png"
---
21.09.21

# Merge Intervals [Sorting, Medium]
from Leetcode

- [start_i, end_i] 로 이뤄진 `interval` 배열의 배열 `intervals`가 주어진다.
- 겹치는 `interval`를 합친 뒤에, 서로 겹치지 않는 `interval`의 배열로 return 하라.

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

```

**Example 2:**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

```

---

## 내 아이디어

- 다음과 같은 `counts` 객체를 만든다.
    - key : start, end 포지션
    - value : start 일 경우 1, end일 경우 -1
- key를 정렬한 다음, 루프를 돌린다.
    - current에 Value 값을 계속 더해준다.
    - Value값을 더해주면서, `current`가 0에서 1이 되는 순간의 포지션을 start에, 1에서 0이 되는 순간을 end에 기록한다.
    - → 해보다 보니 [[0,0] [1,4]] 혹은 [[1,4],[1,4]] 같은 `interval`이 존재해함. 이 때는 0→ 1, 1→0 순간만 기록하는 방법이 먹히지 않음.
    - → 그냥 이전까지 0이었으면 무조건 `start`로 기록. 새로 더했는데 0이면 무조건 `end`로 기록하니까 다시 제대로 작동. (`prev`, `current` 변수 사용)
- `start`와 `end`를 가지고 `results`에 push한다.

## 솔루션 코드

```jsx
var merge = function(intervals) {
    const counts = {};
    intervals.forEach((interval) => {
        counts[interval[0]] = counts[interval[0]] ? counts[interval[0]] + 1 : 1;
        counts[interval[1]] = counts[interval[1]] ? counts[interval[1]] - 1 : -1 ;
    })
    
    let prev = 0, current = 0;
    const results = [];
    const keys = Object.keys(counts).sort((a,b) => a-b);
    
    keys.forEach((key) => {
        current = prev + counts[key];
        if (prev === 0) results.push([key]);
        if (current === 0) results[results.length -1][1] = key;
        prev = current;
    });
    return results
};
```

→ 통과.