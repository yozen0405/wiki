## 解法

- 分治

- trie

- sos dp

- greedy（high bit → low bit）

## 例題

### Maximum AND

???+note "[CF 1721 D. ](https://codeforces.com/problemset/problem/1721/D)"

	??? note "思路"
		D&C

???+note "[2019-pre-pC](https://drive.google.com/file/d/1ECLkM85zf-TS8wMCWiqQP89PNK_b6JZQ/view)"
	給 $n,q$，有筆條件，每筆條件 $(l,r,c)$ 代表 $a_l \oplus \ldots \oplus a_r$ 要是 $c$，構造總和最小的 $a_1, \ldots ,a_n$
    
    $n,q\le 5\times 10^5,0\le c < 2^{30}$
    
	??? note "思路"
		> Subtask 3

        對於一個 interval(l, r)，若存在一個 x 滿足 [x, r] 都要是 0，那我們就將 r 設為 x - 1。處理過後，將每個 interval 以 pair(l, r) 的形式存到到 vector v[r] 裡面，我們從 1 遍歷到 n，假如我們現在到了 i，若 i 一定要填 0，那就跳過，otherwise 若 v[i] 有 pair，我們就檢查從 [l, i) 有沒有已經被選的，如果沒有就選上 i

		> Subtask 4 & 5

        對於每個 bit，都獨立做 Subtask 3，若在處理的過程中多帶一個 log n ，則只會拿到 Subtask 4 的分數，總複雜度 O( (n + q) log C ) 。
	
???+note "[Xor Minimization](https://atcoder.jp/contests/abc281/tasks/abc281_f)"
	給一個長度為 $n$ 的陣列 $a_1,a_2,\ldots ,a_n$，選一個非負整數 $x$，使 $a_i$ 變成 $a_i \oplus x$。輸出陣列的最大值最小可以是多少
	
	$1\le n\le 1.5\times 10^5,0\le a_i < 2^{30}$
	
	??? note "思路"
		從 high bit 到 low bit 考慮，可以觀察到若大家都是 0 或 1 可以直接 greedy 的選，否則我們就要分成兩種情況遞迴下去，然後將比較小的情況多增加 (1 << i)，再將兩種情況取 max
		
		實作上可將全部的點打在 Trie 上面，
		
	??? note "code"
		```cpp linenums="1"
		#include <cstdio>
        #include <iostream>
        #include <algorithm>
        using namespace std;
        int n;
        int son[4500005][2],cnt = 1;
        void insert(int x) {
            int u = 1;
            for (int i = 29; i >= 0; i--) {
                int v = x >> i & 1;
                if (!son[u][v]) son[u][v] = ++cnt;
                u = son[u][v];
            }
        }
        int query(int x, int dep) {
            if (!son[x][0] && !son[x][1]) return 0;
            if (!son[x][0]) return query(son[x][1], dep - 1);
            if (!son[x][1]) return query(son[x][0], dep - 1);
            return min(query(son[x][0], dep - 1),query(son[x][1], dep - 1)) | 1 << dep;
        }
        int main() {
            scanf("%d", &n);
            for (int _ = 1; _ <= n; _++) {
                int a;
                scanf("%d", &a);
                insert(a);
            }
            printf("%d\n",query(1, 29));
            return 0;
        }
        ```
	
???+note "[CF 1847 F. The Boss's Identity](https://codeforces.com/contest/1847/problem/F)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，對於 $i>n$，$(a_{i-n}\mid a_{i-n+1})$。給你 $q$ 筆 query :
	
	- $\text{query}(v):$ 輸出最小的 index $i$ 滿足 $a_i > v$

	$n, q\le 2\times 10^5, 0\le a_i\le 10^9$
	
	??? note "思路"
		性質 : 任意一個 $a_i$ 其實就是原陣列的某一段區間的「或」。可以用打表找出來。

        說明 : 

        令 $a=[1, 2, 3, 4, 5]$，打表 $[1,2,3,4,5,12,23,34,45,512,123,234,345]$

        | 2    | 3    | 4    | 5    |
        | ---- | ---- | ---- | ---- |
        | 12   | 23   | 34   | 45   |
        | 512  | 123  | 234  | 345  |

        可以發現規律恰好是 (n - 1) 一組

        對於一個「環狀」 subarray [l, r]，real_index = (n - 1) * (r - l) + 1 + (r - n)

        贏過的數量 = 一個循環的數量 * 贏過幾組 + 1 + 贏過自己這組的幾個人

        所以問題就變成 : 給定 $v$，在原陣列中找出一段區間，使得區間「或」$>v$。

        考慮從 $i$ 往前的一段子區間，最多只有 $30$ 個不同的結果。這樣就有 $30\times n$ 個子區間了，記錄他們在序列中第一次出現的位置，以及區間或起來的值，對於 query 二分查找即可
		
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

        const int INF = 9e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        void solve() {
            int n, q;
            cin >> n >> q;
            vector<int> a(n);
            for (int i = 0; i < n; i++) {
                cin >> a[i];
            }

            vector<pair<int, int>> f;
            vector<pair<int, int>> info;
            info.pb({1, a[0]});
            for (int i = 0; i < 2 * n; i++) {
                vector<pair<int, int>> g;
                f.pb({0, i});
                for (auto [value, pos] : f) {
                    value |= a[i % n];
                    if (!g.empty() && g.back().F == value) {
                        g.pop_back();
                    }
                    g.pb({value, pos});
                }
                f.swap(g);
                if (i > n) {
                    for (auto [value, pos] : f) {
                        info.pb({(n - 1) * (i - pos) + 1 + i - n, value});
                    }
                }
            }

            sort(info.begin(), info.end());
            vector<pair<int, int>> res;
            for (auto [pos, value] : info) {
                if (res.empty() || value > res.back().F) {
                    res.pb({value, pos});
                }
            }

            while (q--) {
                int v;
                cin >> v;
                auto it = upper_bound(res.begin(), res.end(), make_pair(v, INF));
                if (it == res.end()) {
                    cout << "-1\n";
                } else {
                    cout << it->S << '\n';
                }
            }
        }

        signed main() {
            int t = 1;
            cin >> t;
            while (t--) {
                solve();
            }
        } 
        ```
	
???+note "[CF 1851 F. Lisa and the Martians](https://codeforces.com/contest/1851/problem/F)"
	
### IOIC 

???+note "<a href="/wiki/other/images/ioic_201.html" target="_blank">2023 IOIC 201 . 獨一無二的區間和(ㄏㄢˋ)</a>"
	
	??? note "思路"
		估計：證明 $n > 30$ 時無解
		
		假設有解，令 $b_i = a_1 \, \& \, a_2 \, \& \, \cdots \, a_i$。
		
		1. 根據定義，$b_1, b_2, \ldots, b_{31}$ 必須兩兩不同。
	    
	    2. 因為 $b_{i + 1} = b_i \, \& \, a_{i + 1}，b_{i + 1}$ 是 $b_i$ 的 submask。
	
	    綜上兩點，$b_{i + 1}$ 是 $b_i$ 的 proper submask。這使得
		
		```
	    __builtin_popcount(b[1]) > __builtin_popcount(b[2]) > ... > __builtin_popcount(b[31])
	    ```
	    
	    因為 $0 \leq b_1, b_2, \ldots, b_{31} < 2^{30}$，__builtin_popcount 的值域是 $[0, 30]$
	    $\Rightarrow \,$ `__builtin_popcount(b[i])` = $31 - i$
	    $\Rightarrow \, b_1 = a_1 = 111111111111111111111111111111_2$
	    $\Rightarrow \, a_1 \, \& \, a_2 = a_2 \quad \rightarrow\leftarrow$
	
	    構造：證明 $n \leq 30$ 時總是有解
	    
	    $a_i = 2^{30} - 1 - 2^{i}$ 是個合法構造。

???+note "<a href="/wiki/other/images/ioic_301.html" target="_blank">2023 IOIC 301 . SOS</a>"

	??? note "思路"
		> SOS
		
	    為了解釋方便，我們把 $AND$ 符號改成 $\subseteq$，並且會用二進位數字表示集合。
	
	    令 $N = \lceil \log_2{n+1} \rceil$，也就是 $n$ 的二進位表示中的位數。整題的 $N \leq 20$
	
	    > Subtask 1 ($N \leq 10$)
	    
	    暴力枚舉，$O(3^N)$ 或 $O(4^N)$ 都可以過。
	
	    > Subtask 2 滿分解
	    
	    令 $g(x) = \bigoplus _ {0 \leq y \ , \ x \subseteq y = y} f(y)$
	    則，$f(x) = a_x + \sum _ {0 \leq y < x \ , \ x \subseteq \ y = y} g(y)$
	
	    令 $F[i][j]$ 代表和 $i$ 的前 $n - j$ 個位元相同的所有子集 $y$ 的 $g(y)$ 的和。
	
	    同理，$G[i][j]$ 代表和 $i$ 的前 $n - j$ 個位元相同的所有子集 $y$ 的 $f(y)$ 的和。
	
	    那麼可以枚舉 $i$ 由小到大，先計算所有 $F[i][*]$之後得到 $f(i) = F[i][N] + a_i$，再計算所有 $G[i][*]$。
	
	    轉移是：
	    
	    $$
	    F[i][j] = \begin{cases}
	        F[i][j-1] &,i \& 2^j = 0\\
	        F[i][j-1] + F[i \oplus 2^j][j-1] &,\text{otherwise}
	        \end{cases}
	    $$
	
	    $$
	    G[i][j] = \begin{cases}
	        G[i][j-1] &,i \& 2^j = 0\\
	        G[i][j-1] \oplus G[i \oplus 2^j][j-1] &,\text{otherwise}
	        \end{cases}
	    $$
	
	    複雜度 $O(2^N N)$
	
	??? note "code"
	
	    ```cpp linenums="1"
	    //Challenge: Accepted
	    #pragma GCC optimize("Ofast")
	    #include <bits/stdc++.h>
	    using namespace std;
	    #ifdef zisk
	    void debug(){cout << endl;}
	    template<class T, class ... U> void debug(T a, U ... b){cout << a << " ", debug(b...);}
	    template<class T> void pary(T l, T r) {
	        while (l != r) cout << *l << " ", l++;
	        cout << endl;
	    }
	    #else
	    #define debug(...) 0
	    #define pary(...) 0
	    #endif
	    #define ll long long
	    #define maxn 1050005
	    #define pii pair<int, int>
	    #define ff first
	    #define ss second
	    #define io ios_base::sync_with_stdio(0);cin.tie(0);
	    const ll inf = 1LL<<60;
	    int fs[maxn][21], gs[maxn][21];
	    ll f[maxn], g[maxn];
	    const int se = (1<<30)-1;
	    void madd(int &x, int y) {
	        x = (x + y) & se;
	    }
	    int main() {
	        io;
	        int n;
	        cin >> n;
	        for (int i = 0;i < n;i++) cin >> f[i];
	        for (int i = 0;i < n;i++) {
	            for (int j = 0;j < 20;j++) {
	                if (j) madd(fs[i][j], fs[i][j-1]);
	                if ((i>>j)&1) madd(fs[i][j], fs[i ^ (1<<j)][j]);
	            }
	            f[i] += fs[i][19];
	            f[i] &= se;
	            gs[i][0] ^= f[i];
	            for (int j = 0;j < 20;j++) {
	                if (j) gs[i][j] ^= gs[i][j-1];
	                if ((i>>j)&1) gs[i][j] ^= gs[i ^ (1<<j)][j];
	            }
	            g[i] = gs[i][19];
	            for (int j = 0;j < 20;j++) {
	                madd(fs[i][j], g[i]);
	            }
	        }	
	        cout << f[n-1] << "\n";
	    }
	    ```

???+note "<a href="/wiki/other/images/ioic_305.html" target="_blank">2023 IOIC 305 . 括號國</a>"

	??? note "code"
		```cpp linenums="1"
		#pragma GCC optimize("O3")
	    #pragma GCC target("popcnt")
	    #pragma GCC target("sse,sse2,sse3,ssse3,abm,avx")
	    #include <bits/stdc++.h>
	    using namespace std;
	
	    using ll = long long;
	
	    const int N = 2e5 + 5;
	
	    int n, r;
	    ll ans;
	    string s;
	    int st[N];
	
	    int main() {
	        ios::sync_with_stdio(false), cin.tie(0);
	        cin >> n >> s;
	        s = " " + s;
	        for (int i = 1; i <= n; i++) {
	            if (s[i] == '(') st[++r] = i;
	            else if (s[i] == ')' && r > 0) ans += 1ll * st[r--] * (n - i + 1);
	        }
	        cout << 2 * ans << '\n';
	    }
		```

