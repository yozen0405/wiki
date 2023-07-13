## 介紹

在遇到要去一直開 `new` 指標的時候（動態開點），若發生 TLE 的情況，而記憶體還夠的情況下，可以使用 Memory pool 的技巧。因為電腦在 `new` 的時候必須去電腦裡面找到空的記憶體，這會花時間，不如我們直接先把記憶體都開好直接用。

## 範例

以 CSES - Distinct Values Queries 使用持久化線段樹為例 :

### CSES Distinct Values Queries

???+note "[CSES - Distinct Values Queries](https://cses.fi/problemset/task/1734)"
    給你長度為 $n$ 的陣列 $a_1,...,a_n$，有 $q$ 個查詢
    
    - 輸出 $a_i,...,a_j$ 之間有幾種不同的數字
    
    $n,q\le 2\times 10^5$
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <algorithm>
        #include <iostream>
        #include <utility>
        #include <vector>
    
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()
    
        using namespace std;
    
        struct Node {
            Node* lc = nullptr;
            Node* rc = nullptr;
            int l, r, sum = 0;
    
            Node() {
            }
    
            Node(int l, int r) : l(l), r(r) {
            }
    
            void pull() {
                sum = lc->sum + rc->sum;
            }
        };
    
        Node pool[500000000 / sizeof(Node)];
        int cnt = 0;
    
        struct DS {
            DS(const vector<int>& v) {
                n = v.size();
                roots = vector<Node*>(n + 1, nullptr);
                int maxv = *max_element(ALL(v));
                last = vector<int>(maxv + 1, -1);
                roots[0] = build(0, n - 1);
                for (int i = 0; i < n; i++) {
                    if (last[v[i]] != -1) {
                        roots[i + 1] = update(roots[i], last[v[i]], 0);
                        roots[i + 1] = update(roots[i + 1], i, 1);
                    } else {
                        roots[i + 1] = update(roots[i], i, 1);
                    }
                    last[v[i]] = i;
                }
            }
    
            int query(int l, int r) {
                return query_sum(roots[r], l - 1, r - 1);
            }
    
           private:
            int n;
            vector<Node*> roots;
            vector<int> last;
            // 單點改值 區間查詢
    
            Node* build(int l, int r) {
                Node* root = new (&pool[cnt++]) Node(l, r);
                if (l == r) {
                    return root;
                }
                int mid = (l + r) / 2;
                root->lc = build(l, mid);
                root->rc = build(mid + 1, r);
                root->pull();
                return root;
            }
    
            Node* update(const Node* root, int pos, int val) {
                Node* now = new (&pool[cnt++]) Node(*root);
                if (now->l == now->r) {
                    now->sum = val;
                    return now;
                }
    
                if (pos <= now->lc->r) {
                    now->lc = update(now->lc, pos, val);
                } else {
                    now->rc = update(now->rc, pos, val);
                }
                now->pull();
                return now;
            }
    
            int query_sum(const Node* root, int qL, int qR) {
                if (root->r < qL || qR < root->l) return 0;
                if (qL <= root->l && root->r <= qR) {
                    return root->sum;
                }
                return query_sum(root->lc, qL, qR) + query_sum(root->rc, qL, qR);
            }
        };
    
        signed main() {
            ios::sync_with_stdio(0);
            cin.tie(0);
    
            int n, q;
            cin >> n >> q;
    
            vector<int> a(n);
            for (int i = 0; i < n; i++) {
                cin >> a[i];
            }
    
            vector<int> b = a;
            sort(ALL(b));
            for (int i = 0; i < n; i++) {
                a[i] = lower_bound(ALL(b), a[i]) - b.begin();
            }
    
            DS ds(a);
            while (q--) {
                int l, r;
                cin >> l >> r;
                cout << ds.query(l, r) << '\n';
            }
        }
    	```

這題的記憶體限制是 512 MB（MB 是 Millions Byte）。`Node pool[500000000 / sizeof(Node)]`，500MB / 一個 node 佔用幾個 byte 就可以得到總共能開多少 node，sizeof 可以看一個 Node 佔多少 byte

裡面 `Node* now = new (&pool[cnt++]) Node(*root);` 的語法叫做 placement new，直接指定記憶體位址

