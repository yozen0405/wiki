https://vjudge.net/contest/345697

## 基本操作

### Merge

???+note "code"
	```cpp linenums="1"
    Node* Merge(Node* a, Node* b) {
        if (!a) return b;
        if (!b) return a;

        if (a->pri > b->pri) {
            a->rc = Merge(a->rc, b);
            a->pull();
            return a;
        } else {
            b->lc = Merge(a, b->lc);
            b->pull();
            return b;
        }
    }
    ```
    
### Split

???+note "code"
    ```cpp linenums="1"
	pair<Node*, Node*> Split(Node* root, int val) {
        if (!root) return {nullptr, nullptr};

        if (root->key <= val) {
            auto [A, B] = Split(root->rc, val);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = Split(root->lc, val);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }
    ```

### Split by size

???+note "code"
	```cpp linenums="1"
	// 把一個 treap split 成兩個 treap，滿足左邊的 treap 剛好有 k 個節點，
    // 這 k 個節點是本來 treap 中序輸出的前 k 個節點
    //
    // 左邊 treap 的 key < 右邊 treap 的key
    pair<Node*, Node*> SplitBySize(Node* root, int k) {
        if (!root) return {nullptr, nullptr};

        int cntL;  // 左子樹＋root 節點
        if (root->lc) {
            cntL = root->lc->sz + 1;
        } else {
            cntL = 1;
        }

        if (cntL <= k) {  // root 放左邊
            auto [A, B] = SplitBySize(root->rc, k - cntL);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = SplitBySize(root->lc, k);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }
    ```

## 例題

???+note "[CSES - Cut and Paste](https://cses.fi/problemset/task/2072)"
	給你一個長度為 $n$ 的字母串，$q$ 次將 $(l, r)$ 剪下貼到字母串的尾端，問最後的字母串
	
	$n,q\le 2\times 10^5$

??? note "code 1"
	```cpp linenums="1"
	#include <algorithm>
    #include <cstdlib>
    #include <iostream>
    #include <string>
    #include <utility>

    using namespace std;

    struct Node {
        // int key;
        char val;
        int pri;
        int sz = 1;
        int h = 0;
        Node* lc = nullptr;
        Node* rc = nullptr;

        Node(char val) : val(val), pri(rand()) {
        }
        void pull() {
            h = 0;
            if (lc) h = max(h, lc->h + 1);
            if (rc) h = max(h, rc->h + 1);
            sz = 1;
            if (lc) sz += lc->sz;
            if (rc) sz += rc->sz;
        }
    };

    // 假設 a 的 key 都小於 b 的 key
    Node* Merge(Node* a, Node* b) {
        if (!a) return b;
        if (!b) return a;

        if (a->pri > b->pri) {
            a->rc = Merge(a->rc, b);
            a->pull();
            return a;
        } else {
            b->lc = Merge(a, b->lc);
            b->pull();
            return b;
        }
    }

    // 把一個 treap split 成兩個 treap，滿足左邊的 treap 剛好有 k 個節點，
    // 這 k 個節點是本來 treap 中序輸出的前 k 個節點
    //
    // 左邊 treap 的 key < 右邊 treap 的key
    pair<Node*, Node*> SplitBySize(Node* root, int k) {
        if (!root) return {nullptr, nullptr};

        int cntL;  // 左子樹＋root 節點
        if (root->lc) {
            cntL = root->lc->sz + 1;
        } else {
            cntL = 1;
        }

        if (cntL <= k) {  // root 放左邊
            auto [A, B] = SplitBySize(root->rc, k - cntL);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = SplitBySize(root->lc, k);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }

    /*
    pair<Node*, Node*> Split(Node* root, int val) {
        if (!root) return {nullptr, nullptr};

        if (root->key <= val) {
            auto [A, B] = Split(root->rc, val);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = Split(root->lc, val);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }
    */

    int main() {
        int n, q;
        string str;

        cin >> n >> q;
        cin >> str;

        Node* root = nullptr;
        for (int i = 0; i < n; i++) {
            Node* x = new Node(str[i]);
            root = Merge(root, x);
        }

        while (q--) {
            int l, r;
            cin >> l >> r;
            auto [tmp, C] = SplitBySize(root, r);
            auto [A, B] = SplitBySize(tmp, l - 1);
            root = Merge(A, Merge(C, B));
        }

        for (int i = 0; i < n; i++) {
            auto [x, tmp] = SplitBySize(root, 1);
            cout << x->val;
            root = tmp;
        }

        return 0;
    }
    ```

???+note "[CSES - Substring Reversals](https://cses.fi/problemset/task/2073)"
	給你一個長度為 $n$ 的字母串，$q$ 次 reverse$(l, r)$，問最後的字母串
	
	$n,q\le 2\times 10^5$
	
??? note "code2"
	```cpp linenums="1"
	#include <algorithm>
    #include <cstdlib>
    #include <iostream>
    #include <string>
    #include <utility>

    using namespace std;

    struct Node {
        // int key;
        char val;
        int pri;
        int sz = 1;
        int h = 0;
        bool rev = false;  // 是否要反轉
        Node* lc = nullptr;
        Node* rc = nullptr;

        Node(char val) : val(val), pri(rand()) {
        }
        void pull() {
            h = 0;
            if (lc) h = max(h, lc->h + 1);
            if (rc) h = max(h, rc->h + 1);
            sz = 1;
            if (lc) sz += lc->sz;
            if (rc) sz += rc->sz;
        }
        void push() {
            if (rev) {
                swap(lc, rc);
                if (lc) lc->rev ^= 1;
                if (rc) rc->rev ^= 1;
                rev = false;
            }
        }
    };

    // 假設 a 的 key 都小於 b 的 key
    Node* Merge(Node* a, Node* b) {
        if (!a) return b;
        if (!b) return a;

        if (a->pri > b->pri) {
            a->push();
            a->rc = Merge(a->rc, b);
            a->pull();
            return a;
        } else {
            b->push();
            b->lc = Merge(a, b->lc);
            b->pull();
            return b;
        }
    }

    // 把一個 treap split 成兩個 treap，滿足左邊的 treap 剛好有 k 個節點，
    // 這 k 個節點是本來 treap 中序輸出的前 k 個節點
    //
    // 左邊 treap 的 key < 右邊 treap 的key
    pair<Node*, Node*> SplitBySize(Node* root, int k) {
        if (!root) return {nullptr, nullptr};

        root->push();

        int cntL;  // 左子樹＋root 節點
        if (root->lc) {
            cntL = root->lc->sz + 1;
        } else {
            cntL = 1;
        }

        if (cntL <= k) {  // root 放左邊
            auto [A, B] = SplitBySize(root->rc, k - cntL);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = SplitBySize(root->lc, k);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }

    /*
    pair<Node*, Node*> Split(Node* root, int val) {
        if (!root) return {nullptr, nullptr};

        if (root->key <= val) {
            auto [A, B] = Split(root->rc, val);
            root->rc = A;
            root->pull();
            return {root, B};
        } else {
            auto [A, B] = Split(root->lc, val);
            root->lc = B;
            root->pull();
            return {A, root};
        }
    }
    */

    int main() {
        int n, q;
        string str;

        cin >> n >> q;
        cin >> str;

        Node* root = nullptr;
        for (int i = 0; i < n; i++) {
            Node* x = new Node(str[i]);
            root = Merge(root, x);
        }

        while (q--) {
            int l, r;
            cin >> l >> r;
            auto [tmp, C] = SplitBySize(root, r);
            auto [A, B] = SplitBySize(tmp, l - 1);
            B->rev ^= 1;
            root = Merge(A, Merge(B, C));
        }

        for (int i = 0; i < n; i++) {
            auto [x, tmp] = SplitBySize(root, 1);
            cout << x->val;
            root = tmp;
        }

        return 0;
    }
    ```
    
???+note "[POJ-3580 SuperMemo](https://vjudge.net/problem/POJ-3580)"
	給定一個長度為 N 的序列 `A[]`，M 個以下操作:

    - `ADD x y k` : 將 `A[x, y]` 的每一項都加上 `k`

    - `REVERSE x y` : 將 `A[x, y]` 反轉

    - `REVOLVE x y k` : 將 `A[x, y]` 右旋 `k` 格

    - `INSERT x val` : 將 `val` 插入到 `A[x]` 這一項的後面

    - `DELETE x` : 刪除 `A[x]` 這一項

    - `MIN x y` : 輸出 `A[x, y]` 中的最小值

    $n,m\le 10^6$