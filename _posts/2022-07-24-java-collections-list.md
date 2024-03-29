---
title: Java Collections - List
date: 2022-07-24 19:08:00 +09:00
categories: [Blog, Java, Data Structure]
tags: [data structure java collections]
---

리스트는 객체를 인덱스로 관리하기 때문에 리스트에 객체를 추가하면 인덱스가 자동 부여 된다.

리스트는 인터페이스로 되어 있으며 추가(add), 검색(get, contain, size), 삭제(remove, clear)기능의 메서드로 이루어져 있다.

값 중복을 허용하는 점이 Set과는 다른 점이다.

배열과의 차이는 크기가 동적으로 변한다는 것이다.

배열과는 다르게 엘리먼트들 사이에 빈공간을 허용하지 않으며, 중간 객체가 제거되면 해당 객체 인덱스부터 마지막까지 한칸씩 앞으로 이동한다.

리스트의 종류에는 ArrayList, Vector, LinkedList가 있다.

## ArrayList

- 인덱스를 가지고 있어 검색에 용이
- 중간 부분 삽입/삭제시 비어있는 부분을 매꾸기 때문에 해당 행위가 빈번할 경우 성능 하락
- 동기화 보장 x

## Vector

- 동기화 보장
- 하나의 스레드가 하나의 자원을 이용하는 경우 오히려 성능 저하
- 공간이 모자를 경우 공간*2만큼의 공간을 확보하기 때문에 메모리 많이 먹음

## LinkedList

- 마지막 노드를 검색하기 위해서는 처음부터 찾아가야 해서 검색에 적합하지 않음
- 삽입/삭제시 해당 노드 주소만 바꾸면 되기때문에 삽입/삭제가 빈번할 경우 적합

## ArrayList, Vector, LinkedList 성능 비교

||ArrayList|Vector|LinkedList|
|-----------|:---:|:---:|:---:|
|Thread Safe|No|Yes|No|
|add()|O(N)|O(N)|O(1)|
|get()|O(1)|O(1)|O(N)|
|remove()|O(N)|O(N)|O(1)|
|contains()|O(N)|O(N)|O(N)|

