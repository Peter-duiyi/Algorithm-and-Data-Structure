```
#include<iostream>
#include <vector>
#include <algorithm>
#include <string>
#include <queue>
#include <set>
#include <unordered_set>
#include <math.h>

using namespace std;

/*
n 代表地图大小
0 表示可以通过的格子
1 表示起点
2 表示终点
3 表示障碍物

0 0 0 0 0 0 0 0 0 0
0         
0
0         3
0     1   3   2
0         3
0
0
0
0
0
*/

struct cell
{
	int _x;
	int _y;
	int _type;

	double _f;    //F(n) = G(n) + H(n) 
	double _g;    //G(n)表示由起点到节点n的预估消耗
	double _h;    //H(n)表示节点n到终点的估计消耗

	cell* _parent;
};

auto cmp = [](cell* c1, cell* c2) { return c1->_f < c2->_f; };


#define ROW 10
#define COL 10
cell grid[ROW][COL];

vector<int> direction{ -1, 0, 1, 0, -1 };
unordered_set<cell*> closedList;                    //存放最终可能路径
std::multiset<cell*, decltype(cmp)> openList(cmp);  //存放排好序的格子与对应路径长度，不能用Set，因为可能有重复_f
bool foundDest = false;

//路径估算，曼哈顿距离
int getManhattanDist(const cell& c1, const cell& c2)
{
	return abs(c1._x - c2._x) + abs(c1._y - c2._y);
}

void tracePath(cell* cur)
{
	cell* tmp = cur;
	while (tmp)
	{
		tmp->_type = 5;
		cout << "(" << tmp->_x << ", " << tmp->_y << ")" << endl;
		tmp = tmp->_parent;
	}
}

void printGrid()
{
	cout << "---------------------" << endl;
	for (int i = 0; i < ROW; i++)
	{
		cout << " ";
		for (int j = 0; j < COL; j++)
		{
			cout << grid[i][j]._type << " ";
		}
		cout << endl;
	}
	cout << "---------------------" << endl;
}

void AStar(cell* src, cell* des)
{
	openList.insert(src);

	while (!openList.empty())
	{
		if (foundDest) break;

		//取出F值最小的点
		cell* prev = *openList.begin();

		//将其从openList中移除，放入closedList中
		openList.erase(prev);
		int x = prev->_x;
		int y = prev->_y;
		closedList.insert(prev);

		//找出该元素四周的点，
		for (int i = 0; i < 4; i++)
		{
			int xNew = x + direction[i];
			int yNew = y + direction[i + 1];

			if (xNew >= ROW || xNew < 0 || yNew >= COL || yNew < 0) continue;

			cell* cur = &grid[xNew][yNew];

			if (cur->_type == 2) //终点
			{
				cur->_parent = prev;
				tracePath(cur);
				foundDest = true;
				break;
			}

			if (cur->_type == 3 || closedList.find(cur) != closedList.end())   //该点是障碍物或者在关闭列表中
			{
				continue;
			}
			
			double hNew = getManhattanDist(*cur, *des);
			double gNew = prev->_g + 1.0;
			double fNew = hNew + gNew;

			if (openList.find(cur) == openList.end() || cur->_g > gNew)
			{
				/*
				如果某个相邻的方格已经在 open list 中，则检查这条路径是否更优，
				也就是说经由当前方格 ( 我们选中的方格 ) 到达那个方格是否具有更小的 G值。如果没有，不做任何操作。
				相反，如果 G 值更小，则把那个方格的父亲设为当前方格 ( 我们选中的方格 ) ，
				然后重新计算那个方格的 F 值和 G 值。

				如果child在openList中，用G值来判断是否更新，而不是F值
				*/
				cur->_h = hNew;
				cur->_g = gNew;
				cur->_f = fNew;
				cur->_parent = prev;
				openList.insert(cur);
			}
		}
	}
}

int main() {
	//初始化地图
	for (int i = 0; i < ROW; i++)
	{
		for (int j = 0; j < COL; j++)
		{
			cell& c = grid[i][j];
			c._x = i;
			c._y = j;
			c._type = 0;
			c._f = FLT_MAX;
			c._g = FLT_MAX;
			c._h = FLT_MAX;
			c._parent = nullptr;
		}
	}

	grid[3][5]._type = 3;
	grid[4][5]._type = 3;
	grid[5][5]._type = 3;

	grid[4][3]._type = 1;   //起点
	grid[4][7]._type = 2;   //终点

	printGrid();

	//初始化起点和终点
	cell* src = &grid[4][3];
	cell* des = &grid[4][7];
	src->_h = getManhattanDist(*src, *des);
	src->_g = 0;
	src->_f = src->_h + src->_g;
	
	AStar(src, des);

	printGrid();

	getchar();
	getchar();
}
```
