---
title: Java - Data Structure - Tree
date: 2022-08-02 18:10:00 +09:00
categories: [Blog, Java, Data Structure]
tags: [data structure java collections]
---

비선형 자료구조 중 하나이다.

계층적 구조를 지니고 있다.

## Term

- 노드(node) : 트리에서의 구성 요소
- 루트(root) : 트리 구조 중 최상위 노드
- 간선(edge) : 노드와 노드를 연결하는 선
- 레벨(level) : 트리에서 각각의 층을 나타내는 단어
- 부모 노드(parent node) : 바로 상위에 존재하는 노드
- 자식 노드(child node) : 바로 하위에 존재하는 노드
- 높이(height) : 트리 중 최고 레벨

## Binary Tree

- 자식 노드가 최대 2개인 트리
- `Node` 구현
```java
public class Node {
	private int value;
	private Node leftNode = null;
	private Node rightNode = null;

	public Node(int value) {
		this.value = value;
	}

	public void setValue(int value) {
		this.value = value;
	}

	public void setLeftNode(Node node) {
		this.leftNode = node;
	}

	public void setRightNode(Node node) {
		this.rightNode = node;
	}

	public void getValue() {
		return this.value;
	}

	public Node getLeftNode() {
		return this.leftNode;
	}

	public Node getRightNode() {
		return this.rightNode;
	}
}
```
- `Tree` 구현
```java
public class Tree {
	private Node root;

	public Tree(Node root) {
		this.root = root;
	}
}
```

## Traversal

- 트리 순회(Tree Traversal)에는 전위 순회(Preorder), 중위 순회(Inorder), 후위 순회(Postorder), 레벨 순서 순회(Level-order)

### Preorder Traversal

- 노드 방문
- 왼쪽 서브 트리를 전위 순회
- 오른쪽 서브 트리를 전위 순회
- 깊이 우선 순회(depth-first traversal)
```java
public void preorder(Node node) {
	System.out.println(node.getValue());
	if(node.getLeftNode != null) preorder(node.getLeft());
	if(node.getRightNode != null) preorder(node.getRight());
}
```

### Inorder Traversal

- 왼쪽 서브 트리를 중위 순회
- 노드 방문
- 오른쪽 서브 트리를 중위 순회
- 대칭 순회(symmetric)
```java
public void inorder(Node node) {
	if(node.getLeftNode != null) inorder(node.getLeft());
	System.out.println(node.getValue());
	if(node.getRightNode != null) inorder(node.getRight());
}
```

### Postorder Traversal

- 왼쪽 서브 트리를 후위 순회
- 오른쪽 서브 트리를 후위 순회
- 노드 방문
```java
public void postorder(Node node) {
	if(node.getLeftNode != null) postorder(node.getLeft());
	if(node.getRightNode != null) postorder(node.getRight());
	System.out.println(node.getValue());
}
```
