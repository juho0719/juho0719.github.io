---
title: Java Collections - Queue
date: 2022-07-29 14:20:00 +09:00
categories: [Blog, Java, Data Structure]
tags: [data structure java collections]
---

FIFO(First-In-First-Out) 구조를 가진다.

들어올 때는 enqueue, 나갈 때는 dequeue라고 한다.

큐는 한쪽 끝을 프론트(front)로 정해서 삭제 연산만 처리하고, 나머지 한쪽은 리어(rear)로 정해서 삽입 연산만 한다.

넓이 우선 탐색(BFS)에 사용된다.

## PriorityQueue

- 원소에 우선순위를 부여하여 높은 순으로 먼저 반환
- 이진 트리 구조로 구현

## ArrayDeque

- `Deque`는 양쪽으로 넣고 빼는 것이 가능한 큐


## PriorityQueue, ArrayDeque 성능 비교

||PriorityQueue|ArrayDeque|
|-----------|:---:|:---:|
|offer()|O(log n)|O(1)|
|peek()|O(1)|O(1)|
|poll()|O(log n)|O(1)|


