# [1087 All Roads Lead to Rome (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805379664297984)

Dijkstra

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 200 + 5;
const int INF = 0x3fffffff;

int N, K, S = 0, D;
map<string, int> string2Int;
map<int, string> int2String;
int g[MAX_N][MAX_N] = { 0 };
int weight[MAX_N];
int pre[MAX_N];
int d[MAX_N], w[MAX_N], ns[MAX_N], num[MAX_N];
bool vis[MAX_N] = { false };
vector<int> res;

void dijkstra(int s) {
	fill(d, d + MAX_N, INF);
	fill(w, w + MAX_N, 0);
	fill(num, num + MAX_N, 0);
	fill(ns, ns + MAX_N, 0);
	for (int i = 0; i < N; i++) {
		pre[i] = i;
	}
	d[s] = 0;
	w[s] = weight[S];
	num[s] = 1;
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
		vis[u] = 1;
		for (int v = 0; v < N; v++) {
			if (!vis[v] && g[u][v] != 0) {
				if (d[u] + g[u][v] < d[v]) {
					d[v] = d[u] + g[u][v];
					w[v] = w[u] + weight[v];
					num[v] = num[u];
					ns[v] = ns[u] + 1;
					pre[v] = u;
				}
				else if (!vis[v] && d[u] + g[u][v] == d[v]) {
					num[v] += num[u];
					if (w[u] + weight[v] > w[v]) {
						w[v] = w[u] + weight[v];
						ns[v] = ns[u] + 1;
						pre[v] = u;
					}
					else if (w[u] + weight[v] == w[v]) {
						double uAvg = 1.0 * (w[u] + weight[v]) / (ns[u] + 1);
						double vAvg = 1.0 * w[v] / ns[v];
						if (uAvg > vAvg) {
							ns[v] = ns[u] + 1;
							pre[v] = u;
						}
					}
				}
			}
		}
	}
}

void DFS(int v) {
	if (v == S) {
		cout << int2String[v];
		return;
	}
	DFS(pre[v]);
	cout << "->" << int2String[v];
}

int main() {
	string s;
	cin >> N >> K >> s;
	string2Int[s] = 0;
	int2String[0] = s;
	weight[0] = 0;
	for (int i = 1; i < N; i++) {
		cin >> s >> weight[i];
		string2Int[s] = i;
		int2String[i] = s;
		if (s == "ROM") {
			D = i;
		}
	}
	for (int k = 0; k < K; k++) {
		string s1, s2;
		int c;
		cin >> s1 >> s2 >> c;
		g[string2Int[s1]][string2Int[s2]] = g[string2Int[s2]][string2Int[s1]] = c;
	}
	dijkstra(0);
	cout << num[D] << " " << d[D] << " " << w[D] << " " << w[D] / ns[D] << endl;
	DFS(D);
	return 0;
}

/*
6 7 HZH
ROM 100
PKN 40
GDN 55
PRS 95
BLN 80
ROM GDN 1
BLN ROM 1
HZH PKN 1
PRS ROM 2
BLN HZH 2
PKN GDN 1
HZH PRS 1
Sample Output:
3 3 195 97
HZH->PRS->ROM
*/

```

## 邻接表 + dijkstra + dfs

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 200 + 10;
const int INF = 0x3fffffff;

struct Node {
    int v, dis;
    Node(int _v, int _dis): v(_v), dis(_dis) {}
};
vector<Node> g[MAX_V];
int weight[MAX_V];
int n, m, st, ed;
unordered_map<string, int> str2Int;
unordered_map<int, string> int2Str;
// dijkstra
int d[MAX_V], vis[MAX_V];
vector<int> pre[MAX_V];
// dfs
int num = 0, maxHap = 0, maxAve = 0;
int hap = 0, numNodes = 0;
vector<int> path, tempPath;

void dijkstra(int s) {
    fill(d, d + MAX_V, INF);
    fill(vis, vis + MAX_V, 0);
    d[s] = 0;
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
            if (d[u] + dis < d[v]) {
                d[v] = d[u] + dis;
                pre[v].clear();
                pre[v].push_back(u);
            }
            else if (d[u] + dis == d[v]) {
                pre[v].push_back(u);
            }
        }
    }
}

void dfs(int v) {
    if (v == st) {
        num++;
        if (hap > maxHap || (hap == maxHap && hap / numNodes > maxAve)) {
            maxHap = hap;
            maxAve = hap / numNodes;
            path = tempPath;
        }
    }
    for (int i = 0; i < pre[v].size(); i++) {
        int u = pre[v][i];
        hap += weight[u];
        numNodes++;
        tempPath.push_back(u);
        dfs(u);
        tempPath.pop_back();
        numNodes--;
        hap -= weight[u];
    }
}

int main() {
    string name;
    cin >> n >> m >> name;
    str2Int[name] = 0;
    int2Str[0] = name;
    st = 0;
    for (int i = 1; i < n; i++) {
        cin >> name >> weight[i];
        str2Int[name] = i;
        int2Str[i] = name;
    }
    ed = str2Int["ROM"];
    for (int i = 0; i < m; i++) {
        string c1, c2;
        int cost;
        cin >> c1 >> c2 >> cost;
        int u = str2Int[c1];
        int v = str2Int[c2];
        g[u].push_back(Node(v, cost));
        g[v].push_back(Node(u, cost));
    }
    dijkstra(st);
    hap = weight[ed];
    dfs(ed);
    cout << num << " " << d[ed] << " " << maxHap << " " << maxAve << endl;
    for (int i = path.size() - 1; i >= 0; i--) {
        cout << int2Str[path[i]] << "->";
    }
    cout << "ROM" << endl;
    return 0;
}

/*
Sample Input:
6 7 HZH
ROM 100
PKN 40
GDN 55
PRS 95
BLN 80
ROM GDN 1
BLN ROM 1
HZH PKN 1
PRS ROM 2
BLN HZH 2
PKN GDN 1
HZH PRS 1
Sample Output:
3 3 195 97
HZH->PRS->ROM
*/

```

