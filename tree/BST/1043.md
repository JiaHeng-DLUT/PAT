# [1043 Is It a Binary Search Tree (25 point(s))](https://pintia.cn/problem-sets/994805342720868352/problems/994805440976633856)

|     Submit Time     |  Status  | Score |                           Problem                            | Compiler  | Run Time | User |
| :-----------------: | :------: | :---: | :----------------------------------------------------------: | :-------: | :------: | :--: |
| 9/21/2019, 18:47:02 | Accepted |  25   | [1043](https://pintia.cn/problem-sets/994805342720868352/problems/994805440976633856) | C++ (g++) |   5 ms   | heng |

```c++
#include <bits/stdc++.h>
using namespace std;

struct Node {
    int v;
    Node* l;
    Node* r;
};
int n;
vector<int> origin, preOrder, postOrder;

void insert(Node*& root, int val) {
    if (!root) {
        root = new Node();
        root->v = val;
        root->l = nullptr;
        root->r = nullptr;
        return;
    }
    if (val < root->v) {
        insert(root->l, val);
    }
    else {
        insert(root->r, val);
    }
}

void pre(Node* root) {
    if (!root) {
        return;
    }
    preOrder.push_back(root->v);
    pre(root->l);
    pre(root->r);
}

void preMirror(Node* root) {
    if (!root) {
        return;
    }
    preOrder.push_back(root->v);
    preMirror(root->r);
    preMirror(root->l);
}

void post(Node* root) {
    if (!root) {
        return;
    }
    post(root->l);
    post(root->r);
    postOrder.push_back(root->v);
}

void postMirror(Node* root) {
    if (!root) {
        return;
    }
    postMirror(root->r);
    postMirror(root->l);
    postOrder.push_back(root->v);
}

void show(vector<int> v) {
    for (int i = 0; i < v.size(); i++) {
        printf("%d", v[i]);
        if (i < v.size() - 1) {
            printf(" ");
        }
    }
    printf("\n");
}

int main() {
    Node* root = nullptr;
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        int val;
        scanf("%d", &val);
        origin.push_back(val);
        insert(root, val);
    }
    pre(root);
    if (preOrder == origin) {
        printf("YES\n");
        post(root);
        show(postOrder);
    }
    else {
        preOrder.clear();
        preMirror(root);
        if (preOrder == origin) {
            printf("YES\n");
            postMirror(root);
            show(postOrder);
        }
        else {
            printf("NO\n");
        }
    }
    return 0;
}

```

