# [1111 Online Map (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805358663417856)

## 邻接矩阵

![image.png](https://i.loli.net/2019/09/07/OqUoLMyX9wYt3iS.png)

```c++
#include <iostream>
#include <vector>
using namespace std;
const int MAX_N = 500 + 5;
const int INF = 0x3fffffff;

int N, M;
int dis[MAX_N][MAX_N];
int Time[MAX_N][MAX_N];
int S, D;
int vis[MAX_N], d[MAX_N], w[MAX_N], disPre[MAX_N];
vector<int> disPath;
int t[MAX_N], nodesNum[MAX_N], TimePre[MAX_N];
vector<int> TimePath;

void dijkstraDis(int s) {
	// init
	fill(vis, vis + MAX_N, 0);
	fill(d, d + MAX_N, INF);
	d[s] = 0;
	fill(w, w + MAX_N, INF);
	for (int i = 0; i < MAX_N; i++) {
		disPre[i] = i;
	}

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
			if (!vis[v] && dis[u][v] != INF) {
				if (MIN + dis[u][v] < d[v]) {
					d[v] = MIN + dis[u][v];
					w[v] = w[u] + Time[u][v];
					disPre[v] = u;
				}
				else if (MIN + dis[u][v] == d[v] && w[u] + Time[u][v] < w[v]) {
					w[v] = w[u] + Time[u][v];
					disPre[v] = u;
				}
			}
		}
	}
}

void dfsDisPath(int d) {
	disPath.push_back(d);
	if (d == S) {
		return;
	}
	dfsDisPath(disPre[d]);
}

void dijkstraTime(int s) {
	// init
	fill(vis, vis + MAX_N, 0);
	fill(t, t + MAX_N, INF);
	t[s] = 0;
	fill(nodesNum, nodesNum + MAX_N, INF);
	for (int i = 0; i < MAX_N; i++) {
		TimePre[i] = i;
	}
	for (int i = 0; i < N; i++) {
		int u = -1, MIN = INF;
		for (int j = 0; j < N; j++) {
			if (!vis[j] && t[j] < MIN) {
				u = j;
				MIN = t[j];
			}
		}
		if (u == -1) {
			return;
		}
		vis[u] = 1;
		for (int v = 0; v < N; v++) {
			if (!vis[v] && Time[u][v] != INF) {
				if (t[u] + Time[u][v] < t[v]) {
					t[v] = t[u] + Time[u][v];
					nodesNum[v] = nodesNum[u] + 1;
					TimePre[v] = u;
				}
				else if (t[u] + Time[u][v] == t[v] && nodesNum[u] + 1 < nodesNum[v]) {
					nodesNum[v] = nodesNum[u] + 1;
					TimePre[v] = u;
				}
			}
		}
	}
}

void dfsTimePath(int d) {
	TimePath.push_back(d);
	if (d == S) {
		return;
	}
	dfsTimePath(TimePre[d]);
}

void show(vector<int> v) {
	for (int i = v.size() - 1; i >= 0; i--) {
		cout << v[i];
		if (i > 0) {
			cout << " -> ";
		}
	}
	cout << endl;
}

int main() {
	fill(dis[0], dis[0] + MAX_N * MAX_N, INF);
	fill(Time[0], Time[0] + MAX_N * MAX_N, INF);
	cin >> N >> M;
	for (int m = 0; m < M; m++) {
		int v1, v2, one_way, len, tm;
		cin >> v1 >> v2 >> one_way >> len >> tm;
		dis[v1][v2] = len;
		Time[v1][v2] = tm;
		if (!one_way) {
			dis[v2][v1] = len;
			Time[v2][v1] = tm;
		}
	}
	cin >> S >> D;
	dijkstraDis(S);
	dfsDisPath(D);
	dijkstraTime(S);
	dfsTimePath(D);
	if (disPath == TimePath) {
		printf("Distance = %d; Time = %d: ", d[D], t[D]);
		show(disPath);
	}
	else {
		printf("Distance = %d: ", d[D]);
		show(disPath);
		printf("Time = %d: ", t[D]);
		show(TimePath);
	}
	return 0;
}

/*
Sample Input 1:
10 15
0 1 0 1 1
8 0 0 1 1
4 8 1 1 1
3 4 0 3 2
3 9 1 4 1
0 6 0 1 1
7 5 1 2 1
8 5 1 2 1
2 3 0 2 2
2 1 1 1 1
1 3 0 3 1
1 4 0 1 1
9 7 1 3 1
5 1 0 5 2
6 5 1 1 2
3 5
Sample Output 1:
Distance = 6: 3 -> 4 -> 8 -> 5
Time = 3: 3 -> 1 -> 5
Sample Input 2:
7 9
0 4 1 1 1
1 6 1 1 3
2 6 1 1 1
2 5 1 2 2
3 0 0 1 1
3 1 1 1 3
3 2 1 1 2
4 5 0 2 2
6 5 1 1 2
3 5
Sample Output 2:
Distance = 3; Time = 4: 3 -> 2 -> 5
*/

```

## 邻接表

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 500 + 5;
const int INF = 0x3fffffff;

struct Node {
    int v, dis, t;
    Node(int _v, int _dis, int _t): v(_v), dis(_dis), t(_t) {}
};
vector<Node> g[MAX_V];
int n, m, st, ed;
int d[MAX_V], t[MAX_V], num[MAX_V], vis[MAX_V], pre[MAX_V];
vector<int> path;

void dijkstra_dis(int s) {
    fill(d, d + MAX_V, INF);
    fill(t, t + MAX_V, INF);
    fill(vis, vis + MAX_V, 0);
    d[s] = 0;
    t[s] = 0;
    for(int i = 0; i < n; i++) {
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
            int time = g[u][j].t;
            if (!vis[v]) {
                if (d[u] + dis < d[v]) {
                    d[v] = d[u] + dis;
                    t[v] = t[u] + time;
                    pre[v] = u;
                }
                else if (d[u] + dis == d[v] && t[u] + time < t[v]) {
                    t[v] = t[u] + time;
                    pre[v] = u;
                }
            }
        }
    }
}

void dijkstra_t(int s) {
    fill(t, t + MAX_V, INF);
    fill(num, num + MAX_V, INF);
    fill(vis, vis + MAX_V, 0);
    t[s] = 0;
    num[s] = 0;
    for(int i = 0; i < n; i++) {
        int u = -1, MIN = INF;
        for (int j = 0; j < n; j++) {
            if (!vis[j] && t[j] < MIN) {
                u = j;
                MIN = t[j];
            }
        }
        if (u == -1) {
            return;
        }
        vis[u] = 1;
        for (int j = 0; j < g[u].size(); j++) {
            int v = g[u][j].v;
            int time = g[u][j].t;
            if (!vis[v]) {
                if (t[u] + time < t[v]) {
                    t[v] = t[u] + time;
                    num[v] = num[u] + 1;
                    pre[v] = u;
                }
                else if (t[u] + time == t[v] && num[u] + 1 < num[v]) {
                    num[v] = num[u] + 1;
                    pre[v] = u;
                }
            }
        }
    }
}

void dfs(int v) {
    if (v == st) {
        return;
    }
    dfs(pre[v]);
    path.push_back(v);
}

int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < m; i++) {
        int v1, v2, one_way, len, t;
        scanf("%d%d%d%d%d", &v1, &v2, &one_way, &len, &t);
        g[v1].push_back(Node(v2, len, t));
        if (!one_way) {
            g[v2].push_back(Node(v1, len, t));
        }
    }
    scanf("%d%d", &st, &ed);
    dijkstra_dis(st);
    dfs(ed);
    vector<int> path_dis(path);
    path.clear();
    dijkstra_t(st);
    dfs(ed);
    vector<int> path_time(path);
    if (path_dis == path_time) {
        printf("Distance = %d; Time = %d: %d", d[ed], t[ed], st);
        for (int i = 0; i < path_dis.size(); i++) {
            printf(" -> %d", path_dis[i]);
        }
        printf("\n");
    }
    else {
        printf("Distance = %d: %d", d[ed], st);
        for (int i = 0; i < path_dis.size(); i++) {
            printf(" -> %d", path_dis[i]);
        }
        printf("\n");
        printf("Time = %d: %d", t[ed], st);
        for (int i = 0; i < path_time.size(); i++) {
            printf(" -> %d", path_time[i]);
        }
        printf("\n");
    }
    return 0;
}

/*
Sample Input 1:
10 15
0 1 0 1 1
8 0 0 1 1
4 8 1 1 1
3 4 0 3 2
3 9 1 4 1
0 6 0 1 1
7 5 1 2 1
8 5 1 2 1
2 3 0 2 2
2 1 1 1 1
1 3 0 3 1
1 4 0 1 1
9 7 1 3 1
5 1 0 5 2
6 5 1 1 2
3 5
Sample Output 1:
Distance = 6: 3 -> 4 -> 8 -> 5
Time = 3: 3 -> 1 -> 5
Sample Input 2:
7 9
0 4 1 1 1
1 6 1 1 3
2 6 1 1 1
2 5 1 2 2
3 0 0 1 1
3 1 1 1 3
3 2 1 1 2
4 5 0 2 2
6 5 1 1 2
3 5
Sample Output 2:
Distance = 3; Time = 4: 3 -> 2 -> 5
*/

```

## References

- [1111. Online Map (30)-PAT甲级真题（Dijkstra + DFS）](https://blog.csdn.net/liuchuo/article/details/52487847)

