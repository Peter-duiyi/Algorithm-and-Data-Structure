### UnionFind

[花花酱 Disjoint-set/Union-find Forest - 刷题找工作 SP1](https://www.youtube.com/watch?v=VJnUwsE4fWA)  
https://segmentfault.com/a/1190000012334345

A very interesting explanation of UnionFind  
https://blog.csdn.net/liujian20150808/article/details/50848646

code below from link that I post(only Chinese version)


What is UnionFind?
It's a tree data structure, which helps people solve problems that involves disjoint sets and relevant operations, for example, union and query.

```
// Get the root of u.
	int Find(int u) {
		// Compress the path during traversal
		if (u != parents_[u])
			parents_[u] = Find(parents_[u]);
		return parents_[u];
	}
```
1. Find will return the root of u
2. Find will compress the path in the process of returning the root. If the nodes in the process are not directly connected to root, Find will set it, which will lower the hight of the tree, and we call it compressing the path.


```
bool Union(int u, int v) {
		int pu = Find(u);  // return the root of u
		int pv = Find(v);  // return the root of v
		if (pu == pv) return false;

		// Meger low rank tree into high rank tree
		if (ranks_[pv] < ranks_[pu])
			parents_[pv] = pu;  // set the root of pv to pu
		else if (ranks_[pu] < ranks_[pv])
			parents_[pu] = pv;  // set the root of pu to pv
		else {
			parents_[pv] = pu;  // set the root of pv to pu
			ranks_[pu] += 1;
		}

		return true;
	}
```

Union function:
1. Before truly union two nodes to a edge, we should verify that if they form an edge already. And we do it by finding and comparing their roots. That's what we do at the first three lines of Union Function.
2. Then, just as mentioned in the comments, since two nodes are not connected, we are going to union them into an edge. We pick the root which has the highest rank as the root of new tree and merge the other low rank tree into the new tree.
That's what we do in the rest part of Union function.
