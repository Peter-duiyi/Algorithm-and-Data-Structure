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

int dist[n];
bool visited[n]; // 最优路径节点集合，visited[i] = true表示已经找到从src到i的最短路径
int parent[n];

/*
单源最短路径（无负权边）
Dijkstra算法
https://www.zhihu.com/question/20630094/answer/758191548
时间：O(n^2)，用了双重循环
基本思想：根据初始点，挨个的把离初始点最近的点一个一个找到并加入集合，集合中所有的点的d[i]都是该点到初始点最短路径长度，
由于后加入的点是根据集合S中的点为基础拓展的，所以也能找到最短路径。
*/
void Dijkstra(int src)
{
	//初始化
	for (int i = 0; i < n; i++)
	{
		dist[i] = INT_MAX;
		parent[i] = -1;
		visited[i] = false;
	}
	dist[src] = 0;

	for (int i = 0; i < n; i++)
	{
		//找出没有访问过的，和src距离最近的点
		int v = 0;
		for (int j = 0; j < n; j++)
		{
			if (!visited[j] && dist[j] < dist[v])
			{
				v = j;
			}
		}

		//将该点加入集合
		visited[v] = true;

		//对相邻的，不在集合中的点进行松弛
		//仔细一想，其实松弛操作，是在维护一个全局最优解
		//假设是一个边长2、2、3的三角形，为什么不走2,2边到另外一个顶点，而是走3那条边，
		//就是因为松弛的时候没有将dist中的3改成4，从这个角度看，我觉得松弛操作已经是在维护全局最优解了
		//还是也可以理解为dist中的数值是在不断更新的，每一次更新都说明了之前的更新是局部最优解？
		for (int j = 0; j < n; j++)
		{
			if (!visited[j] && Graph[v][j] != INT_MAX && dist[v] + Graph[v][j] < dist[j])
			{
				dist[j] = dist[v] + Graph[v][j];
				parent[j] = v;
			}
		}
	}
}

/*
优化的Dijkstra算法
1. 用priority_queue自动排序
2. 邻接表存储Graph
时间：
*/

struct queue_element
{

};

priority_queue<queue_element> pq;
void Dijkstra_optimized(int src)
{

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
	Graph[2][1] = 2;
	Graph[1][3] = 3;

	int src = 2;

	Dijkstra(src);

	getchar();
	getchar();
}
```
