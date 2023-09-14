## 介紹

$\text{mex}(S)$ : 回傳最小沒有出現在集合 $S$ 的非負整數

例如 : 

- $\text{mex}( \{1, 2, 4\} ) = 0$

- $\text{mex}( \{0, 1, 2, 4\} ) = 3$

- $\text{mex}( \{0, 1, 2, 3\} ) = 4$

???+note "code"
	```cpp linenums="1"
	int mex(vector<int>& a) {
        int n = a.size();

        vector<bool> v(n + 1, false);
        for (int x : a) {
            if (x <= n) v[x] = true;
        }
    
        for (int i = 0; i <= n; i++) {
            if (v[i] == false) return i;
        }
        return -1;
    }
    ```

???+note "模版測試 [YOJ 1376. Mex](http://infor.ylsh.ilc.edu.tw/problem/1376)"
	給你一個長度為 $n$ 陣列 $a_1,a_2,\ldots ,a_n$，請你求出 $\text{mex}(\{a_1,a_2,\ldots ,a_n\})$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;

        const int INF = 2e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        int mex(vector<int>& a) {
            int n = a.size();

            vector<bool> v(n + 1, false);
            for (int x : a) {
                if (x <= n) {
                    v[x] = true;
                }
            }

            for (int i = 0; i <= n; i++) {
                if (v[i] == false) return i;
            }
            return -1;
        }

        signed main() {
            int n;
            cin >> n;

            vector<int> a(n);
            for (int i = 0; i < n; i++) {
                cin >> a[i];

            }
            cout << mex(a) << '\n';
        } 
        ```
	
## 例題

???+note "區間 mex [洛谷 P4137](https://www.luogu.com.cn/problem/P4137)"
	給一個長度為 n 的序列 $a_1,\ldots ,a_n$，有 $q$ 筆詢問 :
	
	- $\text{mex}(\{a_l,\ldots ,a_r\}):$ 回傳最小沒有出現在 $\{a_l,\ldots ,a_r\}$ 的非負整數

	$n,q\le 2\times 10^5$
	
	??? note "思路"
		離線處理，將 query 按照 $l_i$ 小到大 sort
		
		我們開一顆值域線段樹第 i 項紀錄 i 在當前離線的左界後第一次出現的位置
		
		我們就只要在線段樹上二分到最小的 i 使 max(0, i) > r
		
		複雜度 O((n + q) log n)
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;

        const int INF = 2e18;
        const int maxn = 2e5 + 5;
        const int M = 1e9 + 7;

        struct Node {
            Node *lc = nullptr;
            Node *rc = nullptr;
            int l, r;
            int mx;
            Node(int l, int r) : l(l), r(r) {}

            void pull() {
                mx = max(lc->mx, rc->mx);
            }
        };

        Node* build(int l, int r) {
            Node *root = new Node(l, r);
            if (l == r) {
                root->mx = INF;
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
                root->mx = val;
                return;
            }
            if (pos <= root->lc->r) {
                update(root->lc, pos, val);
            } else {
                update(root->rc, pos, val);
            }
            root->pull();
            return;
        }

        int walk(Node *root, int val) {
            if (root->l == root->r) {
                return root->l;
            }
            if (root->lc->mx > val) {
                return walk(root->lc, val);
            } else {
                return walk(root->rc, val);
            }
        }

        struct Queries {
            int l, r, id;

            bool operator<(const Queries &rhs) const {
                return l < rhs.l;
            }
        };

        int n, q;
        int a[maxn], ans[maxn], ptr[maxn];
        vector<int> pos[maxn];

        signed main() {
            cin >> n >> q;
            Node *root = build(0, maxn);
            for (int i = 0; i < n; i++) {
                cin >> a[i];
                if (pos[a[i]].empty()) {
                    update(root, a[i], i);
                }
                pos[a[i]].pb(i);
            }

            vector<Queries> queries;
            for (int i = 0; i < q; i++) {
                int l, r;
                cin >> l >> r;
                l--, r--;
                queries.pb({l, r, i});
            }
            sort(ALL(queries));

            int prel = 0;
            for (auto [l, r, id] : queries) {
                while (prel < l) {
                    if (ptr[a[prel]] + 1 <= pos[a[prel]].size() - 1) {
                        ptr[a[prel]]++;
                        update(root, a[prel], pos[a[prel]][ptr[a[prel]]]);
                    } else {
                        update(root, a[prel], INF);
                    }
                    prel++;
                }
                ans[id] = walk(root, r);
            }
            for (int i = 0; i < q; i++) {
                cout << ans[i] << '\n';
            }
        } 
        ```

???+note "[2022 花中一模 pE]()"
	
???+note "[Atcoder abc272 E. Add and Mex](https://atcoder.jp/contests/abc272/tasks/abc272_e)"
	
	
???+note "[CF 1844 B. Permutations & Primes](https://codeforces.com/contest/1844/problem/B)"
	問 $1,2,\ldots ,n$ 的 permutation 裡面，最多能有幾個區間裡面的數自 $\text{MEX}$ 起來是質數，輸出這個 permutation
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		首先觀察每個位置會有幾個區間 $(l,r)$ 覆蓋到，以 $n=5$ 為例，每個 $cnt_i=$左邊的數字個數 $\times$ 右邊的數字個數 :
		
		$$cnt=[5,8,9,8,5]$$
		
		如果一個 subarray 裡面沒有 $1$ 那 $\text{MEX}$ 就是 $1$，所以 $1$ 需要放在最多個區間 $(l,r)$ 會覆蓋到的位置，也就是最中間
		
		顯然 $\text{MEX}$ 要是越大的數字越難達到，那不如我們就先考慮比 $1$ 大一點的質數 $2$。我們想要使盡量少的區間覆蓋到 $2$，使得盡量多的區間 $\text{MEX}$ 是 $2$，那麼放在最邊邊一定是最好的，不失一般性假設 $2$ 是放在最前面。
		
		我們來整理一下目前的區間包含 $1,2$ 的情況
		
		1. 沒有 $1$，有 $2$ $\Rightarrow \text{MEX}=1$
		
		2. 沒有 $1$，沒有 $2$ $\Rightarrow \text{MEX}=1$
	
		3. 有 $1$，沒有 $2$ $\Rightarrow \text{MEX}=2$
	
		4. 有 $1$，有 $2$ $\Rightarrow \text{MEX} > 2$
	
		發現前三種情況的 $\text{MEX}$ 已經固定，所以我們要想辦法使第四種的 $\text{MEX}$ 是質數的區間個數盡量多。有 $1$，有 $2$ 的區間一定是 $l=1,r\ge \text{mid}$，而 $3$ 是大於 $2$ 的第一個質數，也就是 $\text{MEX}$ 最好達到的質數，所以我們要使 $l=1,r\ge \text{mid}$ 的區間包含 $3$ 的個數要越少越好，那當然就是把 $3$ 放在陣列的尾端。
	
		實作上將 $2$ 放頭，$3$ 放尾，$1$ 置中，其他數字隨便放，就樣就完成此題了

---

## 資料

- [CSDN Blog](https://blog.csdn.net/jokercold/article/details/78778434?ops_request_misc=&request_id=&biz_id=102&utm_term=mex%20%E5%8C%BA%E9%97%B4%E5%86%85%E6%9C%AA%E5%87%BA%E7%8E%B0%E7%9A%84%E6%9C%80%E5%B0%8F%E6%AD%A3%E6%95%B4%E6%95%B0&utm_medium=distribute.wap_search_result.none-task-blog-2~all~sobaiduweb~default-2-.wap_first_rank_v2_rank_v29&spm=1018.2118.3001.4187)