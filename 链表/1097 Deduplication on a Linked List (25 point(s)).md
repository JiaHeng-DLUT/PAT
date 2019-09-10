# [1097 Deduplication on a Linked List (25 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805369774129152)

1. 可以直接使用 `%05d` 的输出格式，以在不足 `5` 位时在高位补 `0`。但是要注意 `-1` 不能使用 `%05d` 输出，否则会输出 `-0001`（而不是 `-1` 或者 `00001`），因此必须要留意 `-1` 的输出。
2. 题目可能会有无效结点，即不在题目给出的首地址开始的链表。

![image.png](https://i.loli.net/2019/09/10/phkdbFG1rZxE2Ty.png)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 1e5 + 5;

struct Node {
	int address, data, next;
	int order = 2 * MAX_N;	// 结点在链表上的序号，无效结点记为 2 * MAX_N
}nodes[MAX_N];

int vis[MAX_N] = { 0 };

bool cmp(Node a, Node b) {
	return a.order < b.order;
}

int main() {
	int s, N;
	scanf("%d %d", &s, &N);
	for (int n = 0; n < N; n++) {
		int Address, Data, Next;
		scanf("%d %d %d", &Address, &Data, &Next);
		nodes[Address].address = Address;
		nodes[Address].data = Data;
		nodes[Address].next = Next;
	}
	int cntValid = 0, cntRemoved = 0;	// 未删除的有效结点个数和已删除的有效结点个数
	for (int p = s; p != -1; p = nodes[p].next) {
		if (vis[abs(nodes[p].data)] == 0) {
			nodes[p].order = cntValid++;
			vis[abs(nodes[p].data)] = 1;
		}
		else {
			nodes[p].order = MAX_N + cntRemoved++;
		}
	}
	sort(nodes, nodes + MAX_N, cmp);
	int cnt = cntValid + cntRemoved;
	for (int i = 0; i < cnt; i++) {
		if (i != cntValid - 1 && i != cnt - 1) {
			printf("%05d %d %05d\n", nodes[i].address, nodes[i].data, nodes[i + 1].address);
		}
		else {
			printf("%05d %d -1\n", nodes[i].address, nodes[i].data);
		}
	}
	return 0;
}

/*
Sample Input:
00100 5
99999 -7 87654
23854 -15 00000
87654 15 -1
00000 -15 99999
00100 21 23854
Sample Output:
00100 21 23854
23854 -15 99999
99999 -7 -1
00000 -15 87654
87654 15 -1
*/

```







