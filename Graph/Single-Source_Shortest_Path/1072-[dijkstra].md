# [1072 Gas Station (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805396953219072)

> 所有无向边均真实存在，在 Dijkstra 算法的过程中需要考虑所有加油站，因此顶点范围是 `1 ~ (n + m)`。

## 邻接表

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_V = 1e3 + 10 +10;
const int INF = 0x3fffffff;

struct Node {
    int v, dis;
    Node(int _v, int _dis): v(_v), dis(_dis) {}
};
vector<Node> g[MAX_V];
int n, m, k, ds;
int d[MAX_V], vis[MAX_V];

void dijkstra(int s) {
    fill(d, d + MAX_V, INF);
    fill(vis, vis + MAX_V, 0);
    d[s] = 0;
    for (int  i = 0; i < n + m; i++) {
        int u = -1, MIN = INF;
        for (int j = 1; j <= n + m; j++) {
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
            if (!vis[v] && d[u] + dis < d[v]) {
                d[v] = d[u] + dis;
            }
        }
    }
}

int getID(string s) {
    if (s[0] == 'G') {
        return stoi(s.substr(1)) + n;
    }
    else {
        return stoi(s);
    }
}

int main() {
    scanf("%d%d%d%d", &n, &m, &k, &ds);
    for (int i = 0; i < k; i++) {
        string p1, p2;
        int dis;
        cin >> p1 >> p2 >> dis;
        int u = getID(p1);
        int v = getID(p2);
        g[u].push_back(Node(v, dis));
        g[v].push_back(Node(u, dis));
    }
    int index = -1, maxMin = 0, minAve = INF;
    for (int i = n + 1; i <= n + m; i++) {
        dijkstra(i);
        int Min = INF, Ave = 0;
        for (int j = 1; j <= n; j++) {
            if (d[j] > ds) {
                Min = -1;
                break;
            }
            Ave += d[j];
            Min = min(Min, d[j]);
        }
        if (Min != -1) {
            if (Min > maxMin) {
                index = i;
                maxMin = Min;
                minAve = Ave;
            }
            else if (Min == maxMin && Ave < minAve) {
                index = i;
                minAve = Ave;
            }
        }
    }
    if (index == -1) {
        printf("No Solution\n");
    }
    else {
        printf("G%d\n", index - n);
        printf("%.1f %.1f\n", double(maxMin), double(double(minAve) / n));
    }
    return 0;
}
```

## 邻接矩阵

```c++
#include <iostream>
#include <algorithm>
using namespace std;
typedef long long ll;
const int MAX_N = 1e3 + 10 + 5;
const int MAX_V = 1e8;

// input
int N, M, K, DS;
// solve
int g[MAX_N][MAX_N];
int d[MAX_N], vis[MAX_N];
// output 
int res = 0, minDisSum = MAX_V, maxMinDis = 0;

void dijkstra(int s) {
	d[0] = MAX_V;
	for (int i = 1; i <= N + M; i++) {
		d[i] = g[s][i];
		vis[i] = 0;
	}
	for (int j = 1; j <= N + M - 1; j++) {
		int pos = 0;
		for (int i = 1; i <= M + N; i++) {
			if (!vis[i] && d[i] < d[pos]) {
				pos = i;
			}
		}
		if (pos) {
			vis[pos] = 1;
			for (int i = 1; i <= N + M; i++) {
				if (!vis[i] && d[pos] + g[pos][i] < d[i]) {
					d[i] = d[pos] + g[pos][i];
				}
			}
		}
	}
}

int main() {
	cin >> N >> M >> K >> DS;
	// init
	for (int i = 1; i <= N + M; i++) {
		for (int j = 1; j <= N + M; j++) {
			g[i][j] = MAX_V;
		}
	}
	for (int i = 0; i < K; i++) {
		char P1[5], P2[5];
		int p1 = 0, p2 = 0, Dist;
		cin >> P1 >> P2 >> Dist;
		if (P1[0] == 'G') {
			for (int j = 1; j < 5 && P1[j]; j++) {
				p1 = p1 * 10 + P1[j] - '0';
			}
			p1 += N;
		}
		else {
			for (int j = 0; j < 5 && P1[j]; j++) {
				p1 = p1 * 10 + P1[j] - '0';
			}
		}
		if (P2[0] == 'G') {
			for (int j = 1; j < 5 && P2[j]; j++) {
				p2 = p2 * 10 + P2[j] - '0';
			}
			p2 += N;
		}
		else {
			for (int j = 0; j < 5 && P2[j]; j++) {
				p2 = p2 * 10 + P2[j] - '0';
			}
		}
		// cout << p1  << " " << p2 << " " << Dist << endl;
		g[p1][p2] = g[p2][p1] = Dist;
	}
	/*
	for (int i = 1; i <= N + M; i++) {
		for (int j = 1; j <= N + M; j++) {
			cout << g[i][j] << " ";
		}
		cout << endl;
	}
	// */
	// solve
	for (int i = N + 1; i <= N + M; i++) {
		dijkstra(i);
		bool flag = true;
		int disSum = 0, minDis = MAX_V;
		for (int i = 1; i <= N; i++) {
			if (d[i] > DS) {
				flag = false;
				break;
			}
			disSum += d[i];
			if (d[i] < minDis) {
				minDis = d[i];
			}
		}
		if (flag && (minDis > maxMinDis || (minDis == maxMinDis && disSum < minDisSum))) {
			res = i;
			minDisSum = disSum;
			maxMinDis = minDis;
		}
	}
	if (res - N > 0) {
		cout << "G" << res - N << endl;
		cout.precision(1);
		cout << fixed << (double)maxMinDis << " " << (double)minDisSum / N << endl;
	}
	else {
		cout << "No Solution" << endl;
	}
	return 0;
}

/*
Sample Input 1:
4 3 11 5
1 2 2
1 4 2
1 G1 4
1 G2 3
2 3 2
2 G2 1
3 4 2
3 G3 2
4 G1 3
G2 G1 1
G3 G2 2
Sample Output 1:
G1
2.0 3.3
Sample Input 2:
2 1 2 10
1 G1 9
2 G1 20
Sample Output 2:
No Solution
*/

```

## References

- [1072. Gas Station (30)](<https://blog.csdn.net/jmlikun/article/details/50039563>)

