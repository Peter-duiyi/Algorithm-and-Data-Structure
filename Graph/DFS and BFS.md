代码如果存在问题，后续会继续修改

```
#include<iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <queue>
#include <time.h>

using namespace std;

const int n = 10; //节点 0 -（n-1）
int Graph[n][n];

int color[n]; //0表示白色，1表示灰色，2表示黑色
int dist[n];
bool visited[n];
queue<int> q;

int timeStamp;  //时间戳
int d[n];
int f[n];
int parent[n];

/*
注：BFS求单源最短路径(SSSP)时只适用于无权图，因为BFS保证从起点出发经过**最少的边**到达目标顶点的路径就是最短路径。
如果要用BFS计算在有权图上的SSSP，我们可以将每条边展开为由多条长度为1的边组成的路径，但也因此扩大了图的规模，在实际应用中不合理
*/

/*
按照算法导论上的伪代码写的BFS
*/
void BFS(int src) //给定图和源节点
{
	//初始化所有点信息
	for (int i = 0; i < n; i++)
	{
		color[i] = 0;
		dist[i] = INT_MAX;
	}

	//初始化源点信息
	color[src] = 1;
	dist[src] = 0;
	q.push(src);

	while (!q.empty())
	{
		int cur = q.front();
		q.pop();
		//查表
		for (int i = 0; i < n; i++)
		{
			if (Graph[cur][i] == 1 && color[i] == 0)  //点i与cur相邻，并且i没有被发现过
			{
				q.push(i);
				dist[i] = dist[cur] + 1;
				color[i] = 1;
			}
		}
		color[cur] = 2;
	}
}

/*
自己写的简化版
*/
void BFS_Simple(int src)
{
	//初始化
	for (int i = 0; i < n; i++)
	{
		visited[i] = false;
		dist[i] = INT_MAX;
	}
	visited[src] = true;
	q.push(src);
	dist[src] = 0;

	//BFS
	while (!q.empty())
	{
		int cur = q.front();
		q.pop();
		for (int i = 0; i < n; i++)
		{
			if (Graph[cur][i] == 1 && !visited[i])
			{
				dist[i] = dist[cur] + 1;
				visited[i] = true;
				q.push(i);
			}
		}
	}
}

/*
按照算法导论上的伪代码写的DFS
*/
void DFS_Visit(int cur)
{
	color[cur] = 1;
	d[cur] = ++timeStamp;

	for (int i = 0; i < n; i++)
	{
		if (Graph[cur][i] != INT_MAX && color[i] == 0) //点i与cur相邻，并且i没有被发现过
		{
			parent[i] = cur;
			DFS_Visit(i);
		}
	}

	color[cur] = 2;
	f[cur] = ++timeStamp;
}

void DFS()
{
	//初始化
	for (int i = 0; i < n; i++)
	{
		color[i] = 0;
		parent[i] = 0;
	}
	timeStamp = 0;

	for (int i = 0; i < n; i++)
	{
		if (color[i] == 0)
		{
			DFS_Visit(i);
		}
	}
}

                        
int main() {
	//初始化有向图
	for (int i = 0; i < n; i++)
	{
		for (int j = 0; j < n; j++)
		{
			Graph[i][j] = INT_MAX;
		}
	}
	Graph[2][1] = 1;
	Graph[1][3] = 1;

	int src = 2;

	BFS(src);
	//BFS_Simple(src);

	//DFS();

	getchar();
	getchar();
}
                          
```
