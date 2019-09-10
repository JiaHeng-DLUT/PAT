# [1032 Sharing (25 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805460652113920)

1. 使用 `%05d` 格式输出地址，可以使不足 `5` 位的整数的高位补 `0`。
2. 使用 `map` 容易超时。
3. `scanf` 使用 `%c` 格式时是可以读入空格的，因此在输入地址、数据、后继结点地址时，格式不能写成 `%d%c%d`，必须在中间加空格。

![image.png](https://i.loli.net/2019/09/10/Pe5KQ2UyuO1rqVG.png)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX_N = 1e5 + 5;

struct Node {
	char data;
	int next;
	bool flag = false;	// 结点是否在第一条链表中出现
}nodes[MAX_N];

int main() {
	int s1, s2, N;
	scanf("%d %d %d", &s1, &s2, &N);
	for (int n = 0; n < N; n++) {
		int Address, Next;
		char Data;
		scanf("%d %c %d", &Address, &Data, &Next);
		nodes[Address].data = Data;
		nodes[Address].next = Next;
	}
	int p;
	for (p = s1; p != -1; p = nodes[p].next) {
		nodes[p].flag = 1;
	}
	for (p = s2; p != -1; p = nodes[p].next) {
		if (nodes[p].flag) {
			break;
		}
	}
	if (p != -1) {
		printf("%05d\n", p);
	}
	else {
		printf("-1\n");
	}
	return 0;
}

/*
Sample Input 1:
11111 22222 9
67890 i 00002
00010 a 12345
00003 g -1
12345 D 67890
00002 n 00003
22222 B 23456
11111 L 00001
23456 e 67890
00001 o 00010
Sample Output 1:
67890
Sample Input 2:
00001 00002 4
00001 a 10001
10001 s -1
00002 a 10002
10002 t -1
Sample Output 2:
-1
*/

```

