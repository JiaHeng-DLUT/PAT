# [1099 Build A Binary Search Tree (30 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805367987355648)

|     Submit Time     |  Status  | Score |                           Problem                            | Compiler  | Run Time | User |
| :-----------------: | :------: | :---: | :----------------------------------------------------------: | :-------: | :------: | :--: |
| 9/21/2019, 19:15:30 | Accepted |  30   | [1099](https://pintia.cn/problem-sets/994805342720868352/problems/994805367987355648) | C++ (g++) |   2 ms   | heng |

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 100 + 5;

struct  node {
	int v, l, r;
} nodes[MAX_N];
int n;
int inOrder[MAX_N];
int num = 0;
vector<int> res;

void in(int root) {
	if (root < 0) {
		return;
	}
	in(nodes[root].l);
	nodes[root].v = inOrder[num++];
	in(nodes[root].r);
}

void BFS() {
	queue<int> q;
	q.push(0);
	while (!q.empty()) {
		int root = q.front();
		res.push_back(nodes[root].v);
		if (nodes[root].l > 0) {
			q.push(nodes[root].l);
		}
		if (nodes[root].r > 0) {
			q.push(nodes[root].r);
		}
		q.pop();
	}
}

int main() {
	cin >> n;
	for (int i = 0; i < n; i++) {
		cin >> nodes[i].l >> nodes[i].r;
	}
	for (int i = 0; i < n; i++) {
		cin >> inOrder[i];
	}
	sort(inOrder, inOrder + n);
	in(0);
	BFS();
	for (int i = 0; i < res.size(); i++) {
		cout << res[i];
		if (i < res.size() - 1) {
			cout << " ";
		}
	}
	cout << endl;
}

```

