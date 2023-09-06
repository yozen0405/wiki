## 解法

- 分治

- trie

- sos dp

- greedy（high bit → low bit）

## 例題

### Maximum AND

???+note "[CF 1721 D. Maximum AND](https://codeforces.com/problemset/problem/1721/D)"
	給長度為 $n$ 的陣列 $a$ 和 $b$，我們定義 $f(a,b)$ :
	
	- $c_i=a_i \oplus b_i$

	- $f(a,b)=c_1 \mathbin{\&} c_2 \mathbin{\&} \cdots \mathbin{\&} c_n$

	你可以 reorder $b$，輸出 $f(a,b)$ 最大可以到多少
	
	$1\le n\le 10^5$
	
	??? note "思路"
		我們從高位考慮到低位，若 $a_i$ 最高位是 1 的 bit 跟 $b_i$ 最高位是 0 的 bit 一樣多，且 $a_i$ 最高位是 0 的 bit 跟 $b_i$ 最高位是 1 的 bit 一樣多，ans 的最高位就可以是 1，然後我們在分兩組解子問題
		
		不過這其實就是在問相反的數量一不一樣，例如 010 的數量跟 101 的數量一不一樣，如果一樣的化這個 bit 就可以是 1，實作上一樣從高位到低位考慮，例如現在考慮第 i 個 bit，就要去看 ans | (1 << i) 裡有 1 的 bit，a, b 兩邊的數量是否一樣
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>

        using namespace std;

        int main() {
            ios::sync_with_stdio(false); cin.tie(0);
            int t;
            cin >> t;
            while (t--) {
                int n;
                cin >> n;
                vector<int> a(n), b(n);
                for (int& x : a) cin >> x;
                for (int& x : b) cin >> x;

                auto check = [&](int ans) {
                    map<int, int> cnt;
                    for (int x : a) ++cnt[x & ans];
                    for (int x : b) --cnt[~x & ans];
                    bool ok = true;
                    for (auto it : cnt) ok &= it.second == 0;
                    return ok;
                };

                int ans = 0;
                for (int bit = 29; bit >= 0; --bit) 
                    if (check(ans | (1 << bit)))
                        ans |= 1 << bit;

                cout << ans << '\n';
            }
        }
		```

???+note "[2019 全國賽模擬賽 pC. 序列構造 (Sequence)](https://drive.google.com/file/d/1ECLkM85zf-TS8wMCWiqQP89PNK_b6JZQ/view)"
	給 $n,q$，有 $q$ 筆條件，每筆條件 $(l,r,c)$ 代表 $a_l \oplus \ldots \oplus a_r$ 要是 $c$，構造總和最小的 $a_1, \ldots ,a_n$
    
    $n,q\le 5\times 10^5,0\le c < 2^{30}$
    
    ??? note "思路"
    	> Subtask 3
    
        對於一個 interval(l, r)，若存在一個 x 滿足 [x, r] 都要是 0，那我們就將 r 設為 x - 1。處理過後，將每個 interval 以 pair(l, r) 的形式存到到 vector v[r] 裡面，我們從 1 遍歷到 n，假如我們現在到了 i，若 i 一定要填 0，那就跳過，otherwise 若 v[i] 有 pair，我們就檢查從 [l, i) 有沒有已經被選的，如果沒有就選上 i
    
    	> Subtask 4 & 5
    
        對於每個 bit，都獨立做 Subtask 3，若在處理的過程中多帶一個 log n ，則只會拿到 Subtask 4 的分數，總複雜度 O( (n + q) log C ) 。
		
		> 參考自 : [108 學年度 全國資訊學科能力模擬賽 題目講解](https://hackmd.io/@briansu/BkAdtSP6B#pC-%E5%BA%8F%E5%88%97%E6%A7%8B%E9%80%A0-Sequence)
		
???+note "[Atcoder abc281 F. Xor Minimization](https://atcoder.jp/contests/abc281/tasks/abc281_f)"
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

???+note "[CF 1851 F. Lisa and the Martians](https://codeforces.com/contest/1851/problem/F)"
	給 $n,k$，和一個長度為 $n$ 的序列 $a_1,\ldots ,a_n$，問是否存在 $(a_i \oplus x) \& (a_j \oplus x)$ 最大，輸出任意一組 $i,j,x$
	
	$2\le n\le 2\times 10^5, 1\le k\le 30, 0\le a_i,x < 2^k, i\neq j$
	
	??? note "思路"
		我們可以發現要使 $a_i, a_j$ 的 bit 要盡量一樣，是 0, 0 或 1, 1。利用 Trie，去 Greedy 的找即可
		
		---
		
		> 另解 :
		
		將陣列小到大 sort，對於 $a_i$，我們可以發現 index 小於 $i$，且二進制跟 $a_i$ 最像的就是 $a_{i-1}$，因為 $a_i,a_{i-1}$ 可能有一個相等的 prefix，然後接著才是一個 bit $a_i$ 是 $1$，$a_{i-1}$ 是 $0$。有了這個結論，我們就可以將陣列 sort，枚舉相鄰兩項配起來，構造 $x$，取 max 即可
	
## AND

對於一個序列來說，distinct 的區間 AND 最多只會有 $30\times n$ 個

???+note "<a href="/wiki/other/images/ioic_201.html" target="_blank">2023 IOIC 201 . 獨一無二的區間和(ㄏㄢˋ)</a>"
	給 $n$，問是否存在一個序列 $a_1 ,\ldots ,a_n$，使任意區間 AND 是 distinct 的，如果有，要輸出一組解
	
	$1\le n\le 5000,0\le a_i < 2^{30}$
	
	??? note "思路"
		$n > 30$ 時無解
	
	    $n \leq 30$ 時總是有解，且 $a_i = 2^{30} - 1 - 2^{i}$ 是個合法構造。

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
	    
	    > 參考 : [【Codeforces Round #882(Div. 2)题解(A-D,F)】](https://www.bilibili.com/video/BV1zV4y187wq/?share_source=copy_web&t=551)
		
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

### IOIC 

???+note "<a href="/wiki/other/images/ioic_301.html" target="_blank">2023 IOIC 301 . SOS</a>"
	給一個長度為 $n$ 的陣列 $a_0 ,\ldots ,a_{n-1}$。有一個函數 $f$，定義域是 $0$ 到 $n-1$ 的整數，對於一個非負整數 $x$，定義
	
	$$f(x) = a_x + \sum \limits_ {y \subseteq x} \left( \bigoplus \limits_{z \subseteq y} f(z) \right)$$
	
	輸出 $f(n-1)$ 模 $2^{30}$ 的數值。
		
	$1 \leq n \leq 2 ^  {20}, 0 \leq a_i < 2^ {30}$
		
	??? note "思路"
		令 $g(x) = \bigoplus \limits_ {y \subseteq x} f(y)$ 則，$f(x) = a_x + \sum \limits_ {y \subseteq x} g(y)$
		
        因為我們的 mask 是 0 ~ (n-1)，所以我們其實可以用 SOS 的模板概念，將模板的順序改一下，變成如下

        ```cpp
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 20; j++) {
                ....
            }
        }
        ```

        那麼可以枚舉 $i$ 由小到大，先計算利用  sum_g(i) 得到 f(i)，將 f(i) 對 sum_f(i) 的貢獻算進去，再利用 sum_f(i) 計算 g(i)，最後再計算 g(i) 對 sum_g(i) 的貢獻。因為 SOS dp 的迴圈順序顛倒了，所以不能壓成滾動陣列，必須注意。
    
		> 參考自 : <https://hackmd.io/@cychien/SyX3xYJTo>
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long
        #define pb push_back
        using namespace std;

        const int maxn = 1050005;
        const int S = (1 << 30) - 1;
        int sum_g[maxn][21], sum_f[maxn][21];
        int f[maxn], g[maxn];

        void add(int &x, int y) {
            x = (x + y) & S;
        }

        signed main() {
            int n;
            cin >> n;
            for (int i = 0; i < n; i++) {
                cin >> f[i];
            }

            for (int i = 0; i < n; i++) {
                // 計算 f(i)
                for (int j = 0; j < 20; j++) {
                    if (j) add(sum_g[i][j], sum_g[i][j - 1]);
                    if (i & (1 << j)) add(sum_g[i][j], sum_g[i ^ (1 << j)][j]);
                }

                // sum_f(i) ← f(i)
                f[i] += sum_g[i][19];
                f[i] &= S;
                sum_f[i][0] ^= f[i];

                // 計算 g(i)
                for (int j = 0; j < 20; j++) {
                    if (j) sum_f[i][j] ^= sum_f[i][j - 1];
                    if (i & (1 << j)) sum_f[i][j] ^= sum_f[i ^ (1 << j)][j];
                }
                g[i] = sum_f[i][19];

                // sum_g ← g(i)
                for (int j = 0; j < 20; j++) {
                    add(sum_g[i][j], g[i]);
                }
            }
            cout << f[n - 1] << "\n";
        }
    	```

???+note "<a href="/wiki/other/images/ioic_305.html" target="_blank">2023 IOIC 305 . 括號國</a>"
	給一個長度為 $n$ 的括號字串，問所有 substring 的「最長合法括號子序列長度」總和為何？
	
	$1\le n\le 2\times 10^5$
	
	??? note "思路"
		我們使用一般用 stack 括號序列的方式，若我們現在遇到了一個 closing，那前一個 opening 與目前這個 closing 的貢獻就是 opening 往前的長度 $\times$ closing 往後的長度，這些 l, r 在這些範圍內的都會算到我們
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        const int N = 2e5 + 5;
        int st[N];

        signed main() {
            int n;
            string s;
            cin >> n >> s;
            s = "$" + s;

            int ans = 0, r = 0;
            for (int i = 1; i <= n; i++) {
                if (s[i] == '(') st[++r] = i;
                else if (s[i] == ')' && r > 0) ans += 1ll * st[r--] * (n - i + 1);
            }
            cout << 2 * ans << '\n';
        }
		```

