---
title: Java - Data Structure
date: 2022-08-01 19:39:00 +09:00
categories: [Blog, Java, Data Structure]
tags: [data structure java collections]
---

데이터를 메모리상에서 관리하는 방법들이다.

효율적인 자료구조가 성능 좋은 알고리즘의 기반이 된다.

데이터의 효율적 관리는 수행속도와 관련이 있다.

## Array

- 선형으로 데이터를 관리
- 미리 메모리를 할당 받아 사용
- 데이터의 물리적 위치와 논리적 위치가 같음

## LinkedList

- 선형으로 데이터를 관리
- 데이터가 추가될 때마다 메모리를 할당
- 데이터끼리는 주소값으로 연결됨
- 데이터의 물리적 위치와 논리적 위치가 다를 수 있음

## Stack

- 나중에 입력한 데이터가 먼저 출력되는 구조 (Last-In-First-Out)

## Queue

- 가장 먼저 입력된 데이터가 먼저 출력 되는 구조 (First-In-First-Out)

## Tree

- 부모 노드와 자식 노드간의 연결로 이루어져 있음

## Heap

- 최소값과 최대값을 빠르게 찾아내기 위해 완전이진트리 형태로 만들어진 자료구조
- 우선순위 큐로 구현