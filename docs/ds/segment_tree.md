- <https://blog.nowcoder.net/n/27772b54fd3c4968b905303d83138dea?from=nowcoder_improve>

## 掃描線

- 曼哈頓轉雪茄夫距離
- [class 10](https://drive.google.com/file/d/1-KGWKW8z2foucvcgeMJ0j2MABreT3sVF/view)



- 線段樹優化建圖
- https://tioj.ck.tp.edu.tw/problems/1169

## 打架線段樹

???+note "區間數字個數"
	給一個長度為 $n$ 陣列，$q$ 筆詢問 :
	
	- $\text{query}(l,r,x):a_l,\ldots ,a_r$，$x$ 出現的次數
	
	- $\text{update}(i,x):$ 將 $a_i=x$
	
	$n,q\le 2\times 10^5,x\le 10^9$
	
	??? note "思路"
		（靜態）沒 update : vec[x] 放 $a_i=x$ 的所有 $x$
		
		（動態）有 update : DS[x] 支援 
		
		- insert(i)
	
		- erase(i)
	
		- lower_bound(i)
	
		使用 Treap 或 `pb_ds::tree`
	
	??? note "code"
		```cpp linenums="1"
		// using pbds
		tree<pii,null_type,less<pii>,rb_tree_tag,tree_order_statistics_node_update> T;
		
		// update(i, x)
		T.erase({a[i], i});
		a[i] = x;
		T.insert({a[i], i});
		
		// query(l, r, x)
		cout << T.order_of_key(mk(id, r + 1)) - T.order_of_key(mk(id, l)); 
		```

???+note "[資芽 OJ 794 — 區間絕對眾數](https://neoj.sprout.tw/problem/794/)"

    輸入一個長度為 $N$ 的正整數序列 $a_1, \ldots, a_N$，接下來有 $Q$ 筆詢問。
    
    每筆詢問輸入 $l_i, r_i$，輸出區間 $[l_i, r_i]$ 的絕對眾數，若不存在請輸出 $0$。
    
    $N, Q \leq 5 \times 10^5, 1 \leq a_i \leq 5 \times 10^5$
    
    ??? note "思路"
    	<figure markdown>
          ![Image title](./images/3.png){ width="300" }
        </figure>
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #include <bits/extc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()
    
        using namespace std;
        using namespace __gnu_pbds;
    
        tree<pii,null_type,less<pii>,rb_tree_tag,tree_order_statistics_node_update> T;
    
        const int INF = 2e18;
        const int maxn = 5e5 + 5;
        const int M = 1e9 + 7;
    
        struct Node {
            Node* lc = nullptr;
            Node* rc = nullptr;
            int l, r;
            int id = -1, cnt = 0;
    
            Node(int l, int r) : l(l), r(r) {}
    
            void pull () {
                if (lc->cnt == 0) {
                    id = rc->id;
                    cnt = rc->cnt;
                    return;
                } 
                if (rc->cnt == 0) {
                    id = lc->id;
                    cnt = lc->cnt;
                    return;
                }
                if (lc->id == rc->id) {
                    id = lc->id;
                    cnt = lc->cnt + rc->cnt;
                } else {
                    if (lc->cnt > rc->cnt) {
                        id = lc->id;
                        cnt = lc->cnt - rc->cnt;
                    } else {
                        id = rc->id;
                        cnt = rc->cnt - lc->cnt;
                    }
                }
            }
        };
    
        int n, q;
        int a[maxn];
    
        Node* build (int l, int r) {
            Node* root = new Node(l, r);
            if (l == r) {
                root->id = a[l];
                root->cnt = 1;
                return root;
            }
    
            int mid = (l + r) / 2;
            root->lc = build(l, mid);
            root->rc = build(mid + 1, r);
            root->pull();
            return root;
        }
    
        pii query(const Node* root, int ql, int qr) {
            if (qr < root->l || root->r < ql) return {-1, 0};
            if (ql <= root->l && root->r <= qr) {
                return {root->id, root->cnt};
            } 
    
            pii tmp = {-1, 0};
            if (ql <= root->lc->r) {
                pii ret = query(root->lc, ql, qr);
                if (tmp.S == 0) {
                    tmp = ret;
                } else if (ret.S != 0) {
                    if (ret.F == tmp.F) {
                        tmp.S += ret.S;
                    } else {
                        if (ret.S > tmp.S) {
                            tmp.F = ret.F;
                            tmp.S = ret.S - tmp.S;
                        } else {
                            tmp.S = tmp.S - ret.S;
                        }
                    }
                }
            }
            if (root->rc->l <= qr) {
                pii ret = query(root->rc, ql, qr);
                if (tmp.S == 0) {
                    tmp = ret;
                } else if (ret.S != 0) {
                    if (ret.F == tmp.F) {
                        tmp.S += ret.S;
                    } else {
                        if (ret.S > tmp.S) {
                            tmp.F = ret.F;
                            tmp.S = ret.S - tmp.S;
                        } else {
                            tmp.S = tmp.S - ret.S;
                        }
                    }
                }
            }
            return tmp;
        }
    
        void init() {
            cin >> n >> q;
            for (int i = 0; i < n; i++) {
                cin >> a[i];
                T.insert ({a[i], i});
            }
        }
    
        void solve() {
            Node* root = build(0, n - 1);
            while (q--) {
                int l, r;
                cin >> l >> r;
                l--, r--;
    
                auto [id, c] = query(root, l, r);
                if (c == 0) {
                    cout << 0 << '\n';
                    continue;
                }
                int cnt = T.order_of_key(mk(id, r + 1)) - T.order_of_key(mk(id, l)); 
                if (cnt > (r - l + 1) / 2) {
                    cout << id << '\n';
                } else {
                    cout << 0 << '\n';
                }
            }
        }
    
        signed main() {
            int t = 1;
            while (t--) {
                init();
                solve();
            }
        } 
        ```

???+note "[TIOJ 2140. 殿壬愛序列](https://tioj.ck.tp.edu.tw/problems/2140)"
    給一個長度為 $n$ 的序列，請支援三種操作：

    1. 給定 $i,x$，將 $a_i$ 設成 $x$
    2. 給定 $l,r,k$，$\forall l\le i\le r,a_i=\lfloor \frac{a_i}{k} \rfloor$
    3. 給定 $l,r$，輸出 $a_l,a_{l+1},\ldots ,a_r$ 的絕對多數，若不存在輸出 $-1$
    
    $n,q\le 10^5$
    
    ??? note "思路"
    	> 參考 : <https://abc864197532.github.io/2021/02/07/tioj-2140/>
    	
        區間除法的部分，因為 $k = 1$ 不會影響陣列，所以可以視為 $k \geq 2$。當區間的最大值 $>0$ 的時候才繼續遞迴下去。這樣一來每個數最多只會被除 $\log C$ 次。實作上每次將一個點除到 $0$ 需要 $O(\log C\times \log n)$，又每次修改只是單點修改，所以複雜度會是 $O((n + q)\log C \log n)\approx 1.2\times 10^8$。
        
        ```cpp linenums="1"
        // 區間除法代碼
        void division(Node* root, int mL, int mR, int val) {
            if (root->r < mL || mR < root->l) return;
            if (root->l == root->r) {
                root->id /= val;
                root->mx = root->id;
                return;
            }

            if (mL <= root->lc->r && root->lc->mx) {
                division(root->lc, mL, mR, val);
            }
            if (root->rc->l <= mR && root->rc->mx) {
                division(root->rc, mL, mR, val);
            }
            root->pull();
        }
        ```
        
        單點修改，區間絕對眾數上面都有提到，略。
    
    ??? note "實作細節"
    	記得要特判 k = 1
    	
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #include <bits/extc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;
        using namespace __gnu_pbds;

        tree<pii,null_type,less<pii>,rb_tree_tag,tree_order_statistics_node_update> T;

        const int INF = 2e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        struct Node{
            int l, r;
            Node *lc = nullptr;
            Node *rc = nullptr;
            int id = -1, cnt = 0;
            int mx;

            Node (int l, int r) : l(l), r(r) {}

            void pull () {
                if (lc->cnt == 0) {
                    id = rc->id;
                    cnt = rc->cnt;
                } else if (rc->cnt == 0) {
                    id = lc->id;
                    cnt = lc->cnt;
                } else {
                    if (lc->id == rc->id) {
                        id = lc->id;
                        cnt = lc->cnt + rc->cnt;
                    } else {
                        if (lc->cnt > rc->cnt) {
                            id = lc->id;
                            cnt = lc->cnt - rc->cnt;
                        } else {
                            id = rc->id;
                            cnt = rc->cnt - lc->cnt;
                        }
                    }
                }
                mx = max(lc->mx, rc->mx);
            }
        };

        int n, q;
        int a[maxn];

        Node* build (int l, int r) {
            Node* root = new Node(l, r);
            if (l == r) {
                root->id = a[l];
                root->cnt = 1;
                root->mx = a[l];
                return root;
            }

            int mid = (l + r) / 2;
            root->lc = build(l, mid);
            root->rc = build(mid + 1, r);
            root->pull();
            return root;
        }

        void update(Node* root, int pos, int val) {
            if (root->l == root->r) {
                T.erase({root->id, root->l});
                root->id = val;
                root->cnt = 1;
                root->mx = val;
                T.insert({root->id, root->l});
                return;
            }

            if (pos <= root->lc->r) {
                update(root->lc, pos, val);
            } else {
                update(root->rc, pos, val);
            }
            root->pull();
        }

        void division(Node* root, int mL, int mR, int val) {
            if (root->r < mL || mR < root->l) return;
            if (root->l == root->r) {
                T.erase({root->id, root->l});
                root->id /= val;
                root->mx = root->id;
                T.insert({root->id, root->l});
                return;
            }

            if (mL <= root->lc->r && root->lc->mx) {
                division(root->lc, mL, mR, val);
            }
            if (root->rc->l <= mR && root->rc->mx) {
                division(root->rc, mL, mR, val);
            }
            root->pull();
        }

        pii query(const Node* root, int ql, int qr) {
            if (qr < root->l || root->r < ql) return {-1, 0};
            if (ql <= root->l && root->r <= qr) {
                return {root->id, root->cnt};
            } 

            pii tmp = {-1, 0};
            if (ql <= root->lc->r) {
                pii ret = query(root->lc, ql, qr);
                if (tmp.S == 0) {
                    tmp = ret;
                } else if (ret.S != 0) {
                    if (ret.F == tmp.F) {
                        tmp.S += ret.S;
                    } else {
                        if (ret.S > tmp.S) {
                            tmp.F = ret.F;
                            tmp.S = ret.S - tmp.S;
                        } else {
                            tmp.S = tmp.S - ret.S;
                        }
                    }
                }
            }
            if (root->rc->l <= qr) {
                pii ret = query(root->rc, ql, qr);
                if (tmp.S == 0) {
                    tmp = ret;
                } else if (ret.S != 0) {
                    if (ret.F == tmp.F) {
                        tmp.S += ret.S;
                    } else {
                        if (ret.S > tmp.S) {
                            tmp.F = ret.F;
                            tmp.S = ret.S - tmp.S;
                        } else {
                            tmp.S = tmp.S - ret.S;
                        }
                    }
                }
            }
            return tmp;
        }

        void init() {
            cin >> n >> q;
            for (int i = 0; i < n; i++) {
                cin >> a[i];
                T.insert ({a[i], i});
            }
        }

        void solve() {
            Node* root = build(0, n - 1);

            int op;
            while(q--) {
                cin >> op;
                if (op == 1) {
                    int i, x;
                    cin >> i >> x;
                    i--;
                    update(root, i, x);
                } else if (op == 2) {
                    int l, r, k;
                    cin >> l >> r >> k;
                    l--, r--;
                    if (k > 1) division(root, l, r, k);
                } else {
                    int l, r;
                    cin >> l >> r;
                    l--, r--;
                    auto [id, c] = query(root, l, r);
                    if (c == 0) {
                        cout << "-1" << '\n';
                        continue;
                    }
                    int cnt = T.order_of_key(mk(id, r + 1)) - T.order_of_key(mk(id, l)); 
                    if (cnt > (r - l + 1) / 2) {
                        cout << id << '\n';
                    } else {
                        cout << "-1" << '\n';
                    }
                }
            }
        }

        signed main() {
            ios::sync_with_stdio(0);
            cin.tie(0);
            int t = 1;
            //cin >> t;
            while (t--) {
                init();
                solve();
            }
        }
        ```
