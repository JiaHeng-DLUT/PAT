# [1034 Head of a Gang (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805456881434624)

## bfs

|     Submit Time     |  Status  | Score |                           Problem                            | Compiler  | Run Time | User |
| :-----------------: | :------: | :---: | :----------------------------------------------------------: | :-------: | :------: | :--: |
| 9/22/2019, 13:12:44 | Accepted |  30   | [1034](https://pintia.cn/problem-sets/994805342720868352/problems/994805456881434624) | C++ (g++) |  15 ms   | heng |

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 2 * 1e3 + 5;

int n, k;
unordered_map<string, int> str2Int;
unordered_map<int, string> int2Str;
int num = 0;
int g[MAX_N][MAX_N] = { 0 };
int vis[MAX_N] = { 0 };
int cnt, total, head, MAX;
map<string, int> ans;

void bfs(int u) {
    queue<int> q;
    q.push(u);
    vis[u] = 1;
    cnt = 1;
    total = 0;
    head = -1;
    MAX = -1;
    while (!q.empty()) {
        int c = q.front();
        q.pop();
        int sum = 0;
        for (int i = 0; i < num; i++) {
            if (g[c][i]) {
                sum += g[c][i];
                if (!vis[i]) {
                    q.push(i);
                    vis[i] = 1;
                    cnt++;
                }
            }
        }
        total += sum;
        if (sum > MAX) {
            head = c;
            MAX = sum;
        }
    }
}

int main() {
    scanf("%d%d", &n, &k);
    for (int i = 0; i < n; i++) {
        string s1, s2;
        int t;
        cin >> s1 >> s2 >> t;
        if (str2Int.count(s1) == 0) {
            str2Int[s1] = num;
            int2Str[num] = s1;
            num++;
        }
        if (str2Int.count(s2) == 0) {
            str2Int[s2] = num;
            int2Str[num] = s2;
            num++;
        }
        int u = str2Int[s1];
        int v = str2Int[s2];
        g[u][v] += t;
        g[v][u] += t;
    }
    for (int i = 0; i < num; i++) {
        if (!vis[i]) {
            bfs(i);
            if (cnt > 2 && total > 2 * k) {
                ans[int2Str[head]] = cnt;
            }
        }
    }
    cout << ans.size() << endl;
    for (auto it : ans) {
        cout << it.first << " " << it.second << endl;
    }
    return 0;
}

```

## dfs

```c++
#include <iostream>
#include <map>
#include <string>
using namespace std;

int N, K;
const int MAX_N = 2e3 + 5;
int G[MAX_N][MAX_N] = { 0 };
int weight[MAX_N] = { 0 };
map<string, int> string2Int;
map<int, string> int2String;
int numPerson = 0;
map<string, int> Gang;	// head->num
bool vis[MAX_N] = { false };

void DFS(int nowVisit, int& head, int &numMemeber, int &totalValue) {
	numMemeber++;
	vis[nowVisit] = 1;
	if (weight[nowVisit] > weight[head]) {
		head = nowVisit;
	}
	for (int i = 0; i < N; i++) {
		if (G[nowVisit][i] != 0) {
			totalValue += G[nowVisit][i];
			G[nowVisit][i] = G[i][nowVisit] = 0;
			if (!vis[i]) {
				DFS(i, head, numMemeber, totalValue);
			}
		}
	}
}

void DFSTrave() {
	for (int i = 0; i < N; i++) {
		if (!vis[i]) {
			int head = i, numMemeber = 0, totalValue = 0;
			DFS(i, head, numMemeber, totalValue);
			if (numMemeber > 2 && totalValue > K) {
				Gang[int2String[head]] = numMemeber;
			}
		}
	}
}

int change(string s) {
	if (string2Int.count(s)) {
		return string2Int[s];
	}
	else {
		string2Int[s] = numPerson;
		int2String[numPerson] = s;
		return numPerson++;
	}
}

int main() {
	cin >> N >> K;
	for (int n = 0; n < N; n++) {
		string name1, name2;
		int w;
		cin >> name1 >> name2 >> w;
		int id1 = change(name1);
		int id2 = change(name2);
		weight[id1] += w;
		weight[id2] += w;
		G[id1][id2] += w;
		G[id2][id1] += w;
	}
	DFSTrave();
	cout << Gang.size() << endl;
	for (auto it : Gang) {
		cout << it.first << " " << it.second << endl;
	}
	return 0;
}

/*
Sample Input 1:
8 59
AAA BBB 10
BBB AAA 20
AAA CCC 40
DDD EEE 5
EEE DDD 70
FFF GGG 30
GGG HHH 20
HHH FFF 10
Sample Output 1:
2
AAA 3
GGG 3
Sample Input 2:
8 70
AAA BBB 10
BBB AAA 20
AAA CCC 40
DDD EEE 5
EEE DDD 70
FFF GGG 30
GGG HHH 20
HHH FFF 10
Sample Output 2:
0
*/

```

## References

- [算法笔记](https://book.douban.com/subject/26827295/)

