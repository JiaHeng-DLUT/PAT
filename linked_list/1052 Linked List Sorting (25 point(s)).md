# [1052 Linked List Sorting (25 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805425780670464)

1. 可以直接使用 `%05d` 的输出格式，以在不足五位时在高位补 `0`。但是要注意 `-1` 不能使用 `%05d` 输出，否则会输出 `-0001`（而不是 `-1` 或者 `00001`），因此必须要留意 `-1` 的输出。
2. 题目可能会有无效结点，即不在题目给出的首地址开始的链表上。
3. 数据里面还有均为无效的情况，这时就要根据有效结点的个数特判输出 `0 -1`。

![image.png](https://i.loli.net/2019/09/10/G64zQS231BnLa8w.png)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 1e5 + 5;

struct Node {
	int address, data, next;
	int flag = false;	// 结点是否在链表上
}nodes[MAX_N];

bool cmp(Node a, Node b) {
	if (!a.flag || !b.flag) {
		return a.flag > b.flag;
	}
	return a.data < b.data;
}

int main() {
	int N, s;
	scanf("%d %d", &N, &s);
	for (int n = 0; n < N; n++) {
		int Address, Data, Next;
		scanf("%d %d %d", &Address, &Data, &Next);
		nodes[Address].address = Address;
		nodes[Address].data = Data;
		nodes[Address].next = Next;
	}
	int cnt = 0;
	for (int p = s; p != -1; p = nodes[p].next) {
		nodes[p].flag = 1;
		cnt++;
	}
	if (cnt == 0) {
		printf("0 -1\n");
	}
	else {
		sort(nodes, nodes + MAX_N, cmp);
		printf("%d %05d\n", cnt, nodes[0].address);
		for (int i = 0; i < cnt; i++) {
			if (i < cnt - 1) {
				printf("%05d %d %05d\n", nodes[i].address, nodes[i].data, nodes[i + 1].address);
			}
			else {
				printf("%05d %d -1\n", nodes[i].address, nodes[i].data);
			}
		}
	}
	return 0;
}

/*
Sample Input:
5 00001
11111 100 -1
00001 0 22222
33333 100000 11111
12345 -1 33333
22222 1000 12345
Sample Output:
5 12345
12345 -1 00001
00001 0 11111
11111 100 22222
22222 1000 33333
33333 100000 -1
*/

```

