# [1030 Travel Plan (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805464397627392)

## Dijkstra

### 邻接矩阵

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 500 + 5;
const int INF = 0x3fffffff;

int n, m, st, ed;
int g[MAX_V][MAX_V] = { 0 };
int cost[MAX_V][MAX_V] = { 0 };
int d[MAX_V], c[MAX_V], pre[MAX_V], vis[MAX_V];

void dijkstra(int s) {
	fill(d, d + n, INF);
	fill(c, c + n, INF);
	fill(vis, vis + n, 0);
	d[s] = 0;
	c[s] = 0;
	for (int i = 0; i < n; i++) {
		int u = -1, MIN = INF;
		for (int j = 0; j < n; j++) {
			if (!vis[j] && d[j] < MIN) {
				u = j;
				MIN = d[j];
			}
		}
		if (u == -1) {
			return;
		}
		vis[u] = true;
		for (int v = 0; v < n; v++) {
			if (!vis[v] && g[u][v] != 0) {
				if (d[u] + g[u][v] < d[v]) {
					d[v] = d[u] + g[u][v];
					c[v] = c[u] + cost[u][v];
					pre[v] = u;
				}
				else if (d[u] + g[u][v] == d[v]) {
					if (c[u] + cost[u][v] < c[v]) {
						c[v] = c[u] + cost[u][v];
						pre[v] = u;
					}
				}
			}
		}
	}
}

void dfs(int v) {
	if (v == st) {
		cout << v << " ";
		return;
	}
	dfs(pre[v]);
	cout << v << " ";
}

int main() {
	cin >> n >> m >> st >> ed;
	for (int i = 0; i < m; i++) {
		int c1, c2, d, c;
		cin >> c1 >> c2 >> d >> c;
		g[c1][c2] = g[c2][c1] = d;
		cost[c1][c2] = cost[c2][c1] = c;
	}
	dijkstra(st);
	dfs(ed);
	cout << d[ed] << " " << c[ed] << endl;
	return 0;
}
```

### 邻接表

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 500 + 5;
const int INF = 0x3fffffff;

struct Node {
	int v, dis, cost;
	Node(int _v, int _dis, int _cost) : v(_v), dis(_dis), cost(_cost) {}
};
vector<Node> g[MAX_V];
int n, m, st, ed;
int d[MAX_V], c[MAX_V], pre[MAX_V], vis[MAX_V];

void dijkstra(int s) {
	fill(d, d + MAX_V, INF);
	fill(c, c + MAX_V, INF);
	fill(vis, vis + MAX_V, 0);
	d[s] = 0;
	c[s] = 0;
	for (int i = 0; i < n; i++) {
		int u = -1, MIN = INF;
		for (int j = 0; j < n; j++) {
			if (!vis[j] && d[j] < MIN) {
				u = j;
				MIN = d[j];
			}
		}
		if (u == -1) {
			return;
		}
		vis[u] = 1;
		for (int j = 0; j < g[u].size(); j++) {
			int v = g[u][j].v;
			int dis = g[u][j].dis;
			int cost = g[u][j].cost;
			if (!vis[v]) {
				if (d[u] + dis < d[v]) {
					d[v] = d[u] + dis;
					c[v] = c[u] + cost;
					pre[v] = u;
				}
				else if (d[u] + dis == d[v]) {
					if (c[u] + cost < c[v]) {
						c[v] = c[u] + cost;
						pre[v] = u;
					}
				}
			}
		}
	}
}

void dfs(int u) {
	if (u == st) {
		printf("%d ", u);
		return;
	}
	dfs(pre[u]);
	printf("%d ", u);
}

int main() {
	scanf("%d%d%d%d", &n, &m, &st, &ed);
	for (int i = 0; i < m; i++) {
		int c1, c2, dis, cost;
		scanf("%d%d%d%d", &c1, &c2, &dis, &cost);
		g[c1].push_back(Node(c2, dis, cost));
		g[c2].push_back(Node(c1, dis, cost));
	}
	dijkstra(st);
	dfs(ed);
	printf("%d %d\n", d[ed], c[ed]);
	return 0;
}
```

## Dijkstra + DFS

![image.png](https://i.loli.net/2019/09/03/JdWhwvAZ8TcmF75.png)

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int N, M, S, D;
const int MAX_N = 500 + 5;
const int INF = 0x3fffffff;
int G[MAX_N][MAX_N] = { 0 };
int cost[MAX_N][MAX_N] = { 0 };
int d[MAX_N], minCost = INF;
bool vis[MAX_N] = { false };
vector<int> pre[MAX_N];
vector<int> tempPath, path;	// 临时路径，最优路径

void dijkstra(int s) {
	fill(d, d + N, INF);
	d[s] = 0;
	for (int i = 0; i < N; i++) {
		int u = -1, MIN = INF;
		for (int j = 0; j < N; j++) {
			if (!vis[j] && d[j] < MIN) {
				u = j;
				MIN = d[j];
			}
		}
		if (u == -1) {
			return;
		}
		vis[u] = true;
		for (int v = 0; v < N; v++) {
			if (!vis[v] && G[u][v] != 0) {
				if (d[u] + G[u][v] < d[v]) {
					d[v] = d[u] + G[u][v];
					vector<int>().swap(pre[v]);
					pre[v].push_back(u);
				}
				else if (d[u] + G[u][v] == d[v]) {
					pre[v].push_back(u);
				}
			}
		}
	}
}

void DFS(int v) {
	if (v == S) {
		tempPath.push_back(v);
		int tempCost = 0;
		for (int i = tempPath.size() - 1; i > 0; i--) {
			int id = tempPath[i], idNext = tempPath[i - 1];
			tempCost += cost[id][idNext];
		}
		if (tempCost < minCost) {
			minCost = tempCost;
			path = tempPath;
		}
		tempPath.pop_back();
		return;
	}
	tempPath.push_back(v);
	for (int i = 0; i < pre[v].size(); i++) {
		DFS(pre[v][i]);
	}
	tempPath.pop_back();
}

int main() {
	cin >> N >> M >> S >> D;
	for (int m = 0; m < M; m++) {
		int c1, c2, d, c;
		cin >> c1 >> c2 >> d >> c;
		G[c1][c2] = G[c2][c1] = d;
		cost[c1][c2] = cost[c2][c1] = c;
	}
	dijkstra(S);
	DFS(D);
	for (int i = path.size() - 1; i >= 0; i--) {
		cout << path[i] << " ";
	}
	cout << d[D] << " " << minCost << endl;
	return 0;
}

/*
Sample Input:
4 5 0 3
0 1 1 20
1 3 2 30
0 3 4 10
0 2 2 20
2 3 1 20
Sample Output:
0 2 3 3 40
*/

```
