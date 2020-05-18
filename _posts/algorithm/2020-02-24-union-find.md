---
title: "[자료구조] Union-Find를 이용한 서로소 집합(Disjoint Set) 구현"
layout: post
tags: 알고리즘 자료구조
---


## 서로소 집합이란?
서로소 집합(Disjoint set)은 공통 원소가 없는 두 집합이다.
 - {1, 2, 3}과 {4, 5, 6}은 서로소이다.
 - {1, 2, 3}과 {3, 4, 5}는 서로소가 아니다.


## Union-find란?
 - 서로소 집합을 구현하기 위한 트리 자료구조다.
 - 서로 다른 두 노드 A, B가 같은 집합에 속하는지 확인하기 위해 최상위 부모노드를 확인한다.
 - 만약 부모 노드가 같다면 두 노드는 같은 집합에 있다는 것을 알 수 있다.

## 구현
 - 각 노드의 부노 노드를 저장하기 위한 **parent** 배열을 선언한다.
 - 특정 노드의 루트 노드를 찾기 위한 함수 **find**를 구현한다. 
 - 두 집합을 합치기 위한 함수 **union**을 구현한다.

아래의 코드는 Union find를 구현한 가장 기본적인 코드다.
```java
class UnionFind {
	int[] parent;
	
	public UnionFind(int size) {
		parent = new int[size];
		for(int i=0; i<size; i++) {
			parent[i] = i;
		}
	}
	public int find(int node) {
		if(parent[node] == node) {
			return node;
		}
		return find(parent[node]);
	}
	public void union(int node1, int node2) {
		int root1 = find(node1);
		int root2 = find(node2);
		if(root1 == root2) return;
		parent[root2] = root1;
	}
}
```

## 최적화

### 경로 압축(Path Compression)
우리가 필요로 하는 부모 노드의 정보는 오직 최상위 부모 노드다. 하지만 트리의 깊이가 깊어지면 부모 노드를 찾기 위해 트리의 깊이만큼 find 함수를 호출한다. 

find 함수를 호출할 때 탐색한 노드의 최상위 부모 노드를 업데이트 해주면 문제를 해결할 수 있다. 이를 **경로 압축(Path Compression)**이라 한다.

```java
public int find(int node) {
	if(parent[node] == node) {
		return node;
	}
	return parent[node] = find(parent[node]);
}
```

### Union by rank
union 함수에서 트리를 합칠 때 높이가 낮은 트리를 높은 트리에 합치는 것이 효율적이다. 그렇지 않다면 트리의 깊이가 깊어져 트리를 탐색하는데 더 많은 시간이 소요될 것이다.

rank 배열에 트리의 높이를 저장하여 높이가 낮은 트리를 높이가 높은 트리에 합치도록 할 수 있다.
```java
class UnionFind {
	int[] parent;
	int[] rank;
	
	public UnionFind(int size) {
		for(int i=0; i<size; i++) {
			rank[i] = 0;
		}
	}
	public void union(int node1, int node2) {
		int root1 = find(node1);
		int root2 = find(node2);
		if(root1 == root2) return;
		
		if(rank[root1] < rank[root2]) {
     		parent[root1] = root2;
		} else {
			parent[root2] = root1;
		}
		// 트리의 높이가 같다면 트리의 높이를 1 높여준다.
		if(rank[root1] == rank[root2]) {
			rank[root1]++;
		}
	}
}
```

## reference
https://ko.wikipedia.org/wiki/서로소_집합_자료_구조
https://gmlwjd9405.github.io/2018/08/31/algorithm-union-find.html