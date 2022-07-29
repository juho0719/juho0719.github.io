---
title: Java Collections - Set
date: 2022-07-29 21:49:00 +09:00
categories: [Blog, Java, Data Structure]
tags: [data structure java collections]
---

저장된 값의 순서가 없고, 중복을 허용하지 않는다.

## HashSet

- 해쉬 알고리즘을 사용
- 내부적으로 `HashMap` 인스턴스를 이용하여 값을 저장

## LinkedHashSet

- `HashSet`의 특징을 그대로 이어받지만, 앞 뒤 요소의 정보를 갖고 있고 순서대로 저장

## TreeSet

- 이진 검색 트리(Binary Search Tree)를 사용하여 자료를 관리

## HashSet, TreeSet 성능 비교

||HashSet|LinkedHashSet|TreeSet|
|-----------|:---:|:---:|:---:|
|add()|O(1)|O(1)|O(h/N)|
|contains()|O(1)|O(1)|O(1)|
|next()|O(log n)|O(log n)|O(log n)|


