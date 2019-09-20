# [1018 Public Bike Management (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805489282433024)

> 本题不能只使用 `Dijkstra` 来解决，因为 `minSend` 和 `minTakeBack` 在路径上的传递不满足最优子结构（不是简单的相加过程）。**只有当一条完整的路径被确定后，才能计算这条路径的 `send` 和 `takeBack`。**

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 500 + 5;
const int INF = 0x3fffffff;

struct Node {
    int v, dis;
    Node(int _v, int _dis) : v(_v), dis(_dis) {}
};
vector<Node> g[MAX_V];
int weight[MAX_V];
int c_max, n, ed, m;
int d[MAX_V], w[MAX_V], vis[MAX_V];
vector<int> pre[MAX_V], path, tempPath;
int minSend = INF, minTakeBack = INF;

void dijkstra(int s) {
    fill(d, d + MAX_V, INF);
    fill(vis, vis + MAX_V, 0);
    d[s] = 0;
    for (int i = 0; i <= n; i++) {
        int u = -1, MIN = INF;
        for (int j = 0; j <= n; j++) {
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
            if (!vis[v]) {
                if (d[u] + dis < d[v]) {
                    d[v] = d[u] + dis;
                    pre[v].clear();
                    pre[v].push_back(u);
                } else if (d[u] + dis == d[v]) {
                    pre[v].push_back(u);
                }
            }
        }
    }
}

void dfs(int v) {
    if (v == 0) {
        int send = 0, takeBack = 0;
        for (int i = tempPath.size() - 1; i >= 0; i--) {
            int id = tempPath[i];
            if (weight[id] > 0) {
                takeBack += weight[id];
            } else {
                if (takeBack >= -weight[id]) {
                    takeBack += weight[id];
                } else {
                    send += -weight[id] - takeBack;
                    takeBack = 0;
                }
            }
        }
        if (send < minSend) {
            minSend = send;
            minTakeBack = takeBack;
            path = tempPath;
        } else if (send == minSend && takeBack < minTakeBack) {
            minTakeBack = takeBack;
            path = tempPath;
        }
        return;
    }
    tempPath.push_back(v);
    for (int i = 0; i < pre[v].size(); i++) {
        dfs(pre[v][i]);
    }
    tempPath.pop_back();
}

int main() {
    scanf("%d%d%d%d", &c_max, &n, &ed, &m);
    c_max /= 2;
    for (int i = 1; i <= n; i++) {
        scanf("%d", &weight[i]);
        weight[i] -= c_max;
    }
    for (int i = 0; i < m; i++) {
        int u, v, dis;
        scanf("%d%d%d", &u, &v, &dis);
        g[u].push_back(Node(v, dis));
        g[v].push_back(Node(u, dis));
    }
    dijkstra(0);
    dfs(ed);
    printf("%d 0", minSend);
    for (int i = path.size() - 1; i >= 0; i--) {
        printf("->%d", path[i]);
    }
    printf(" %d\n", minTakeBack);
    return 0;
}

/*
Sample Input:
10 3 3 5
6 7 0
0 1 1
0 2 1
0 3 3
1 3 1
2 3 1
Sample Output:
3 0->2->3 0

Sample Input:
10 4 4 5
4 8 9 0
0 1 1
1 2 1
1 3 2
2 3 1
3 4 1
Sample Output:
1 0->1->2->3->4 2
*/

```

