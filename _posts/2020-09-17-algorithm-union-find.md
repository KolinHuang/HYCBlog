---
title: 并查集
author: Kol Huang
date: 2020-09-17 14:14:00 +0800
categories: [Blogging, Algorithms]
tags: [Algorithms]
comments: true
math: true
---



> 作用：用于图快速查找祖先的数据结构。

## 简单实现

```java
class UnionFind{
    int[] pre;
    public UnionFind(int n){
        pre = new int[n];
      	//初始化并查集，初始时，所有节点都是孤立的，即有n个连通分支
        for(int i = 0; i < n; ++i){
            pre[i] = i;
        }
    }

  	//找到一个节点对应的连通分支的根节点，即pre[x] = x的节点
    //顺带做了路径压缩，最后把x的父节点修改为了根节点
    public int find(int x){
        if(pre[x] != x){
            pre[x] = find(pre[x]);
        }
        return pre[x];
    }
		//合并两个连通分支
  	//随意指定一个分支的根节点为另一个分支根节点的子节点
    public void union(int a, int b){
      	int x = find(a);
      	int y = find(b);
      	if(x != y)
        		pre[x] = y;
    }
}
```

并查集只适合求**无需具体路径**的连通问题。若要求记录下两个点间的连通路径，就应该用DFS做。