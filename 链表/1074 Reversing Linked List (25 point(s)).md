# [1074 Reversing Linked List (25 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805394512134144)

1. **要考虑可能存在无效结点的情况，即不是由题目给出的头结点引出的单链表上的结点，这些结点是要去掉的，最终不予输出。**
2. 反转链表只改变结点的 `next` 地址，而不会改变本身的地址，因此 `address` 和 `data` 可以视为绑定的。
3. `%05d` 的输出格式会使 `-1` 的输出出现问题，因此一定要将 `-1` 的输出跟其他地址的输出分开来考虑。

![image.png](https://i.loli.net/2019/09/10/AKki6fQVUqGpl1O.png)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 1e5 + 5;

struct Node {
	int address, data, next;
	int order = MAX_N;	// 结点在链表上的序号，无效结点记为 MAX_N
}nodes[MAX_N];

bool cmp(Node a, Node b) {
	return a.order < b.order;
}

int main() {
	int s, N, K;
	scanf("%d %d %d", &s, &N, &K);
	for (int n = 0; n < N; n++) {
		int Address, Data, Next;
		scanf("%d %d %d", &Address, &Data, &Next);
		nodes[Address].address = Address;
		nodes[Address].data = Data;
		nodes[Address].next = Next;
	}
	int cnt = 0;
	for (int p = s; p != -1; p = nodes[p].next) {
		nodes[p].order = cnt++;;
	}
	N = cnt;
	sort(nodes, nodes + MAX_N, cmp);
	for (int i = 0; i < N / K; i++) {	// 枚举完整的 N / K 块
		for (int j = (i + 1) * K - 1; j > i * K; j--) {	// 第 i 块倒着输出
			printf("%05d %d %05d\n", nodes[j].address, nodes[j].data, nodes[j - 1].address);
		}
		// 对每一块的最后一个结点的 next 地址的处理
		printf("%05d %d ", nodes[i * K].address, nodes[i * K].data);
		if (i < N / K - 1) {	// 如果不是最后一块，指向下一块的最后一个结点
			printf("%05d\n", nodes[(i + 2) * K - 1].address);
		}
		else {	// 是最后一块时
			// i = N / K - 1
			if (N % K == 0) {	// 恰好是最后一个结点，输出 -1
				printf("-1\n");
			}
			else {	// 剩下不完整的块按原先顺序输出
				printf("%05d\n", nodes[(i + 1) * K].address);
				// （i + 1) * K = (N / K - 1 + 1) * K = N / K * K
				for (int j = N / K * K; j < N; j++) {
					if (j < N - 1) {
						printf("%05d %d %05d\n", nodes[j].address, nodes[j].data, nodes[j + 1].address);
					}
					else {
						printf("%05d %d -1\n", nodes[j].address, nodes[j].data);
					}
				}
			}
		}
	}
	return 0;
}

/*
Sample Input:
00100 6 4
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
Sample Output:
00000 4 33218
33218 3 12309
12309 2 00100
00100 1 99999
99999 5 68237
68237 6 -1

Sample Input:
00000 6 3
00000 1 11111
11111 2 22222
22222 3 -1
33333 4 44444
44444 5 55555
55555 6 -1
Sample Output:
22222 3 11111
11111 2 00000
00000 1 -1

Sample Input:
00100 6 2
00000 4 99999
00100 1 12309
68237 6 -1
33218 3 00000
99999 5 68237
12309 2 33218
Sample Output:
12309 2 00100
00100 1 00000
00000 4 33218
33218 3 68237
68237 6 99999
99999 5 -1
*/

```

