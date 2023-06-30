<https://drive.google.com/file/d/1wFrDpXJt_OEo3H5Cvwl6W29ZPMPqcy95/view>

???+note "[CSES Range Queries and Copies](https://cses.fi/problemset/task/1737)"
	有一個長度 $n$ 陣列，$q$ 筆以下操作：
    
    - 對於第 $k$ 個陣列單點改值
    
    - 對於第 $k$ 個陣列區間求和
    
    - 複製第 $k$ 個陣列，並將其添加到列表的尾端
    
    $n,q\le 2\times 10^5,a_i\le 10^9$
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <iostream>
        #include <vector>
    
        #define int long long
    
        using namespace std;
    
        struct Node {
            int val = 0;
            int l, r;  // range 左右界
            Node* lc = nullptr;
            Node* rc = nullptr;
    
            void pull() {
                val = lc->val + rc->val;
            }
        };
    
        Node* build(int l, int r) {
            Node* root = new Node;
            root->l = l;
            root->r = r;
            if (l == r) return root;
    
            int mid = (l + r) / 2;
            root->lc = build(l, mid);
            root->rc = build(mid + 1, r);
            return root;
        }
    
        // return 新的 segment tree 的 root
        Node* update(const Node* root, int pos, int val) {
            Node* now = new Node(*root);
    
            if (now->l == now->r) {
                now->val = val;
                return now;
            }
            if (pos <= now->lc->r) {
                now->lc = update(root->lc, pos, val);
            } else {
                now->rc = update(root->rc, pos, val);
            }
            now->pull();
            return now;
        }
    
        int query(const Node* root, int ql, int qr) {
            if (ql <= root->l && root->r <= qr) return root->val;
            if (root->r < ql || qr < root->l) return 0;
            return query(root->lc, ql, qr) + query(root->rc, ql, qr);
        }
    
        signed main() {
            cin.tie(0);
            cin.sync_with_stdio(0);
    
            int n, q;
            cin >> n >> q;
    
            vector<Node*> roots(2);
            roots[1] = build(1, n);
            for (int i = 1; i <= n; i++) {
                int x;
                cin >> x;
                roots[1] = update(roots[1], i, x);
            }
    
            for (int cmd; cin >> cmd && q > 0; q--) {
                if (cmd == 1) {
                    int k, pos, val;
                    cin >> k >> pos >> val;
                    roots[k] = update(roots[k], pos, val);
                } else if (cmd == 2) {
                    int k, ql, qr;
                    cin >> k >> ql >> qr;
                    int ans = query(roots[k], ql, qr);
                    cout << ans << '\n';
                } else {  // cmd == 3
                    int k;
                    cin >> k;
                    roots.push_back(roots[k]);
                }
            }
            return 0;
        }
        ```

???+note "持久化 Treap [NPSC 2014 pD](/wiki/ds/images/NPSC2014.pdf#page=11)"
	給一個長度為 $n$ 個子母串 $s_1,\ldots ,s_n$，以及 $m$ 筆操作 :
	
	- 輸出 $s_l,\ldots ,s_r$

	- 複製 $s_l,\ldots ,s_r$，貼到原本 $s_r$ 之後

	- reverse $s_l,\ldots ,s_r$

	$n,m\le 4\times 10^4$

???+note "[洛谷 P3834 - 【模板】可持久化线段树 2](https://www.luogu.com.cn/problem/P3834)"
	給長度為 $n$ 的序列，$q$ 筆詢問
	
	- $\text{query(}a_l\sim a_r,k):$ 回答 $a_l\sim a_r$ 中第 $k$ 小的數值是多少<br>
	
	$n,q\le 2\times 10^5,|a_i|\le 10^9$
	
	??? note "思路"
		對於每個 $i$ 維護陣列 $v_i$，對於每個 $v_i$ 先等於 $v_{i-1}$，然後再將 $v_i[a_i]$++
		
		<figure markdown>
          ![Image title](./images/1.png){ width="300" }
        </figure>
		
		這樣問一個區間的時候可將 $v_r-v_{l-1}$ 得到一個陣列，在上面 walk
	
		<figure markdown>
          ![Image title](./images/2.png){ width="300" }
        </figure>
        
        實作上因為 0-base 的時候 $l-1$ 會是負的，所以我們將 $v_i$ 的 index 向右平移（詳見代碼）
	
	??? note "code"
		```cpp linenums="1"
		#include <algorithm>
        #include <iostream>
        #include <vector>

        using namespace std;

        struct Node {
            int sum;
            int l, r;
            Node(int sum, int l, int r) : sum(sum), l(l), r(r) {
            }
            Node* lc = nullptr;
            Node* rc = nullptr;

            void pull() {
                sum = lc->sum + rc->sum;
            }
        };

        struct DS {
            DS(const vector<int>& v) {
                n = v.size();
                roots = vector<Node*>(n + 1, nullptr);
                int minv = *min_element(v.begin(), v.end());
                int maxv = *max_element(v.begin(), v.end());
                roots[0] = build(minv, maxv);
                for (int i = 0; i < n; i++) {
                    roots[i + 1] = update(roots[i], v[i], 1);
                }
            }

            int query(int l, int r, int k) {
                Node* p = roots[l];
                Node* q = roots[r + 1];
                while (q->l != q->r) {
                    int cnt = q->lc->sum - p->lc->sum;
                    if (cnt >= k) {
                        p = p->lc;
                        q = q->lc;
                    } else {
                        p = p->rc;
                        q = q->rc;
                        k -= cnt;
                    }
                }
                return q->l;
            }

           private:
            int n;
            vector<Node*> roots;

            Node* build(int l, int r) {
                Node* root = new Node(0, l, r);
                if (l == r) return root;
                int mid = (l + r) / 2;
                root->lc = build(l, mid);
                root->rc = build(mid + 1, r);
                return root;
            }

            // copy on write update
            Node* update(const Node* root, int pos, int val) {
                Node* u = new Node(*root);
                if (root->l == root->r) {
                    u->sum += val;
                    return u;
                }
                if (pos <= root->lc->r) {
                    u->lc = update(root->lc, pos, val);
                } else {
                    u->rc = update(root->rc, pos, val);
                }
                u->pull();
                return u;
            }
        };

        int main() {
            cin.tie(0);
            cin.sync_with_stdio(0);

            int n, q;
            cin >> n >> q;
            vector<int> a(n);
            for (int i = 0; i < n; i++) cin >> a[i];

            vector<int> b = a;
            sort(b.begin(), b.end());
            for (int i = 0; i < n; i++) {
                a[i] = lower_bound(b.begin(), b.end(), a[i]) - b.begin();
            }

            DS ds(a);
            while (q--) {
                int l, r, k;
                cin >> l >> r >> k;
                l--, r--;  // to 0-base
                int rk = ds.query(l, r, k);
                cout << b[rk] << '\n';
            }
            return 0;
        }
        ```

???+note "[CSES - Distinct Values Queries](https://cses.fi/problemset/task/1734)"
    給你長度為 $n$ 的陣列 $a_1,...,a_n$，有 $q$ 個查詢
    
    - 輸出 $a_i,...,a_j$ 之間有幾種不同的數字

	$n,q\le 2\times 10^5$

??? note "模板"
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
        // x->push() 的前提是 x 已經是新的節點
        void push() {
            if (rev) {
                swap(lc, rc);
    
                if (lc) lc = new Node(*lc);
                if (rc) rc = new Node(*rc);
    
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
    
        if (rand() % (a->sz + b->sz) < a->sz) { // a 當 root 的機率是 sz(a) / ( sz(a) + sz(b) )
            a = new Node(*a);
            a->push();
            a->rc = Merge(a->rc, b);
            a->pull();
            return a;
        } else {
            b = new Node(*b);
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
    
        root = new Node(*root);
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