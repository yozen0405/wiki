## 介紹
### 算法精神
先想單一詢問的時候可以怎麼利用值域二分搜

一定會在「答案」所在的值域進行分治，我們將答案的值域列為 $[l,r]$

同時也需要維護在當前分治到的值域下的詢問編號，我們把它叫做 $[ql,qr]$

若詢問並沒有單調性，那就必須自己跑過 $ql\sim qr$，再開兩個陣列將他們分成左右兩類

若詢問有單調性，尋找切點，直接分治 (連結 : dp 優化 - 決策性單調)

我們假設 $[ql,qr]$ 的切點叫做 $t$， $\displaystyle\text{mid}=\frac{l+r}{2}$

$$
\texttt{solve (l, r, qL, qR)}=\begin{cases} \texttt{solve (l, mid, qL, t)} \\ \texttt{solve (mid + 1, r, t + 1, qR)}\end{cases}
$$
### 範例
#### 靜態區間 k 小
???+note "[洛谷 P3834 - 【模板】可持久化线段树 2](https://www.luogu.com.cn/problem/P3834)"
	給長度為 $n$ 的序列，$q$ 筆詢問
	
	- $\text{query(}a_l\sim a_r,k):$ 回答 $a_l\sim a_r$ 中第 $k$ 小的數值是多少<br>
	
	範圍 : $n,q\le 2\times 10^5$


#### 動態區間 k 小
???+note "[洛谷 P2617 - Dynamic Rankings](https://www.luogu.com.cn/problem/P2617)"
	給長度為 $n$ 的序列，$q$ 筆詢問
	
	- $\text{query(}a_l\sim a_r,k):$ 回答 $a_l\sim a_r$ 中第 $k$ 小的數值是多少<br>
	- $\text{modify(}a_i,x):$ 將 $a_i$ 的數值改成 $x$
	
	範圍 : $n,q\le 2\times 10^5$
	
	??? note "思路"
		1. 把原先 $a_i$ 的貢獻給扣除<br>
		2. 將 $x$ 的貢獻加入
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	    #define pb push_back
	    #define mk make_pair
	    #define lowbit(x) (x & (-x))
	    #define F first
	    #define S second
	    #define ALL(x) x.begin(), x.end()
	
	    using namespace std;
	    using PQ = priority_queue<int, vector<int>, greater<int>>;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    int n, m, cnt = 0, tot = 0;;
	    int a[maxn], ans[maxn];
	
	    struct query {
	        int type, x, y, k, id;
	        // 0, l, r, k, qry id
	        // 1, index, number, 1/-1 add or del, qry id
	    };
	
	    query q1[2 * maxn], q2[2 * maxn], q[2 * maxn];
	    query qry[maxn];
	
	    struct BIT {
	        vector<int> bit;
	
	        void init () {
	            bit.resize (n + 1);
	        }
	
	        void add (int x, int d) {
	            while (x <= n) {
	                bit[x] += d;
	                x += lowbit (x);
	            }
	        }
	
	        int query (int x) {
	            int ret = 0;
	            while (x > 0) {
	                ret += bit[x];
	                x -= lowbit (x);
	            }
	            return ret;
	        }
	    } bit;
	
	    void divide (int l, int r, int qL, int qR) {
	        if (l > r || qL > qR) return;
	        if (l == r) {
	            for (int i = qL; i <= qR; i++) {
	                if (q[i].type == 0) {
	                    ans[q[i].id] = l;
	                } 
	            }
	            return;
	        }
	
	        int mid = (l + r) / 2;
	
	        int cnt1 = 0, cnt2 = 0;
	        for (int i = qL; i <= qR; i++) {
	            if (q[i].type == 0) {
	                int t = bit.query (q[i].y) - bit.query (q[i].x - 1);
	                if (q[i].k <= t) q1[++cnt1] = q[i];
	                else q[i].k -= t, q2[++cnt2] = q[i];
	            } 
	            else {
	                if (q[i].y <= mid) {
	                    bit.add (q[i].x, q[i].k); // q[i].x
	                    q1[++cnt1] = q[i];
	                }
	                else q2[++cnt2] = q[i];
	            }
	        }
	
	        // undo
	        for (int i = 1; i <= cnt1; i++) 
	            if (q1[i].type == 1) bit.add (q1[i].x, -q1[i].k);
	        for (int i = 1; i <= cnt1; i++) q[qL + i - 1] = q1[i];
	        for (int i = 1; i <= cnt2; i++) q[qL + cnt1 + i - 1] = q2[i];
	
	        divide (l, mid, qL, qL + cnt1 - 1);
	        divide (mid + 1, r, qL + cnt1, qR);
	    }
	
	    void solve () {
	        cin >> n >> m;
	
	        int x;
	        for (int i = 1; i <= n; i++) {
	            cin >> x;
	            a[i] = x;
	            q[++cnt] = {1, i, a[i], 1, -1};
	        }
	
	        for (int i = 1; i <= m; i++) {
	            int x, y, k;
	            char type;
	            cin >> type;
	            if(type == 'Q') {
	                cin >> x >> y >> k;
	                q[++cnt] = 0, x, y, k, ++tot};
	            }
	            else {
	                cin >> x >> y;
	                q[++cnt] = {1, x, a[x], -1, 0};
	                q[++cnt] = {1, x, a[x] = y, 1, 0};
	            }
	        }
	
	        bit.init ();
	
	        divide (-2e9, 2e9, 1, cnt);
	
	        for (int i = 1; i <= tot; i++) {
	            cout << ans[i] << "\n";
	        }
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            solve();
	        }
	    } 
	    ```

## 例題

### APCS 真假子圖
???+note "[zerojudge g598. 4. 真假子圖](https://zerojudge.tw/ShowProblem?problemid=g598)"
	有 $n$ 個點，$m$ 個 $\texttt{pair}(x,y)$ 代表 $x$ 跟 $y$ 在不同組
		
	再給你 $p$ 組資料，每組資料有 $k$ 個 $\texttt{pair}(x,y)$ 代表 $x$ 跟 $y$ 在不同組
	
	你要輸出哪幾筆資料跟原本的 $m$ 個 $\texttt{pair}$ 產生矛盾
	
	題目保證輸入的資料兩兩之間不矛盾
	
	??? note "思路"
	
	    > 法 1 : 
	
	    技巧 : <a href="/wiki/graph/MST/#3" target="_blank">最大邊最小化生成樹 法3</a>
	
	    考慮找第一個出錯的地方，$\displaystyle \text{mid}=\frac{l+r}{2}=t$
	
	    檢查只用 $\le t$ 的資料聯集是否矛盾
	
	    - 若矛盾，代表 $ans\le t$，刪掉後面的，少一半
	    - 若沒矛盾，代表 $ans> t$，將前面的二分圖縮點，少一半
	
	    複雜度 : $\displaystyle T(p)=T(\frac{p}{2})+O(p\times k)\Rightarrow O(p\times k)$
	
	    > 法 2 : rollback DSU
	
	    註 : 如果資料兩兩之間可以矛盾也是可以做的
	
	    將第 $i$ 個資料的 $k$ 個 $\texttt{pair}$ 加進 DSU，判斷，roll back
	
	    複雜度 : $O(p\times k \times \log C)$

### Atcoder Stamp Rally

???+note "[Atcoder AGC002 D - Stamp Rally](https://atcoder.jp/contests/agc002/tasks/agc002_d)"
	給 $n$ 點 $m$ 邊無向圖，邊的編號 $1 \sim m$
	
	$q$ 筆詢問 $x, y, z$
	
	回答從 $x$ 點出發和從 $y$ 點走的「點集聯集大小」至少是 $z$ 的最大邊編號最小值
	
	- $n,m,q \le 10^5$
	
	??? note "思路"
		> 暴力作法
	
		我們二分搜 $\text{mid}=t$ 
		
		檢查如果只走 $\le t$ 的邊 : 
		
		- $x$ 和 $y$ 是否在同一個連通塊
	
		- 連通塊大小是否 $\ge z$
		
		複雜度 : $O(q\times (n+m))$
		
		--- 
		
		這邊我們引入一個技巧，下面的方法會用到
		
		技巧詳見 : <a href="/wiki/graph/MST/#3" target="_blank">最大邊最小化生成樹 法3</a>
		
		> 方法一 : 把 graph 拆半，兩個子問題圖都只有本來的一半
		
		- $ans \le t$ 少一半的 else
		
		- $ans > t$ 少一半的 else，縮點
		
		時間複雜度 : $O(m \log m)$
		
		空間複雜度 : $O(m)$[^1]
		
		> 方法二 : 存 $\log m$ 個 $n\text{-vertex graph}$
		
		<figure markdown>
	      ![Image title](./images/4.png){ width="500" }
	    </figure>

		時間複雜度 : $O(m \log m + q)=O(m\log m)$
		
		空間複雜度 : $O(m \log m)$

### 區間 gcd

???+note "原創 - 區間 gcd"
	給一個正整數陣列，查詢有幾個區間的 $\gcd = 1$ 
	
	- $O(n\log n)$ 
	
	??? note "思路"
		這題其實就直接 sparse table 預處理，two pointer 維護即可
		
		但是我們還是可以試試看以整體二分搜的觀點切入
		
		---
		對於每個 $l$，看最近的 $r$ 使得 $\gcd (a_l,\cdots a_r)=1$
		
		> 方法1 : 整體二分
		
		假設目前我們在處理左界 $=[ql,qr]$ 的這些 query，他們的右界在 $[l, r]$ 這個範圍
		
		$\displaystyle \text{mid}=\frac{l+r}{2}$，左界在 $[\text{mid}+1,r]$ 這些範圍的都可以往右遞迴
		
		再來考慮左界在 $[ql, \text{mid}]$ 的這些 query
		
		我們要尋找切點 $p$ 使得左界在 $p$ 這個位置他的右界剛好 $\ge \text{mid}$ 
		
		$\texttt{solve (ql, p - 1, l, mid)},\texttt{solve (p, qr, mid + 1, r)}$
		
		> 方法2 : 倍增法
	
		假設 $n=32$，我們考慮第 $i$ 個 query
		
		我們令當前合法區間為 $[i, r_i]$
		
		看能不能將 $r_i$ 往右移動 $32$ 步
		
		看能不能將 $r_i$ 往右移動 $16$ 步
		
		看能不能將 $r_i$ 往右移動 $8$ 步
		
		$\vdots$
		
		??? code "虛擬碼"
			```cpp linenums="1"
			init r[i] = i - 1, g[i] = 0;
			
			for (d = n, n / 2, n / 4, ...)
				build v[i] = gcd (a[i],..., a[i + d - 1])
				
				for i = 1 ~ Q :
					if gcd (g[i], v[r[i] + 1]) != 1 :
						g[i] = gcd (g[i], v[r[i] + 1])
						r_i = r_i + d
			```


### 成大賽 身分調查

???+note "[2023 成大賽初賽 pD.身分調查](https://codeforces.com/gym/437848/problem/D)"
	依序給你 $K$ 個 $\texttt{pair}(x_i,y_i)$ 代表 $x$ 跟 $y$ 在不同組
	
	已知編號 $1$ 的組別，求移除 $[l,r]$ 的 $\texttt{pair}$ 滿足
	
	1. 剩下的 $\texttt{pair}$ 還是能確定 $X$ 的組別
	2. $[l,r]$ 長度最大
	3. 若還是有多組解，輸出左界比最小的
	
	求 $l,r$


### BOI 2020 Joker

???+note "[LOJ #3334. 「BalticOI 2020」小丑](https://loj.ac/p/3334)"
	給你 $n$ 點 $m$ 邊的無向圖，邊以 $1\sim m$ 編號，有 $q$ 筆詢問，第 $i$ 筆詢問問
	
	- 移除編號在 $[l_i,r_i]$ 內的邊是否可以讓圖沒有奇環
	
	範圍 : $n,m,q\le 2\times 10^5$
	
	??? note "思路"
		- <https://www.cnblogs.com/chasedeath/p/14504709.html>
		- <https://www.cnblogs.com/stinger/p/15970444.html>
	
	??? note "code"
		```cpp linenums="1"
		#include <cstdio>
	    #include <algorithm>
	    #include <cctype>
	    using namespace std;
	    typedef pair <int, int> Pii;
	    #define mp make_pair
	    #define rep(i,a,b) for(int i=a,i##end=b;i<=i##end;++i)
	    #define drep(i,a,b) for(int i=a,i##end=b;i>=i##end;--i)
	
	    char IO;
	    int rd() {
	        int s = 0, f = 0;
	
	        while (!isdigit(IO = getchar()))
	            f |= IO == '-';
	
	        do
	            s = (s << 1) + (s << 3) + (IO^'0');
	
	        while (isdigit(IO = getchar()));
	
	        return f ? -s : s;
	    }
	
	    const int N = 2e5 + 10, INF = 1e9 + 10;
	
	    int n, m, q;
	    int U[N], V[N];
	
	    int SX[N * 2], SY[N * 2], C[N], F[N], S[N], T, cnt;
	    pair <int, int> Find(int x) {
	        int c = 0;
	
	        while (x != F[x])
	            c ^= C[x], x = F[x];
	
	        return mp(x, c);
	    }
	    void Union(int &x, int &y) {
	        Pii fx = Find(x), fy = Find(y);
	
	        if (fx.first == fy.first) {
	            y = fx.second == fy.second, x = 0;
	            cnt += y;
	            return;
	        }
	
	        x = fx.first, y = fy.first;
	
	        if (S[x] > S[y])
	            swap(x, y);
	
	        F[x] = y, S[y] += S[x], C[x] = fx.second ^ fy.second ^ 1;
	    }
	    void Back() {
	        int x = SX[T], y = SY[T--];
	
	        if (!x) {
	            cnt -= y;
	            return;
	        }
	
	        F[x] = x, C[x] = 0, S[y] -= S[x];
	    }
	
	    int ans[N];
	    void Add(int i) {
	        SX[++T] = U[i], SY[T] = V[i];
	        Union(SX[T], SY[T]);
	    }
	
	    void Solve(int l, int r, int L, int R) {
	        if (l > r)
	            return;
	
	        while (r > R)
	            ans[r] = r - 1, r--;
	
	        if (L == R) {
	            rep(i, l, r) {
	                if (cnt)
	                    ans[i] = L;
	
	                Add(i);
	            }
	            rep(i, l, r) Back();
	            return;
	        }
	
	        int mid = (L + R + 1) >> 1, p = r + 1;
	        drep(i, R, mid + 1) Add(i);
	        rep(i, l, min(r, mid + 1)) {
	            if (cnt) {
	                p = i;
	                break;
	            }
	
	            Add(i);
	        }
	        rep(i, l, p - 1) Back();
	        Add(mid);
	        Solve(l, p - 1, L, mid - 1);
	        Back();
	        drep(i, R, mid + 1) Back();
	        rep(i, l, p - 1) Add(i);
	        Solve(p, r, mid, R);
	        rep(i, l, p - 1) Back();
	    }
	
	    int main() {
	        n = rd(), m = rd(), q = rd();
	        rep(i, 1, n) F[i] = i, S[i] = 1;
	        rep(i, 1, m) U[i] = rd(), V[i] = rd();
	        rep(i, 1, m) Add(i);
	
	        if (!cnt) {
	            rep(i, 1, q) puts("NO");
	            return 0;
	        }
	
	        rep(i, 1, m) Back();
	        Solve(1, m, 1, m);
	        rep(i, 1, q) {
	            int l = rd(), r = rd();
	            puts(ans[l] >= r ? "YES" : "NO");
	        }
	    }
	    ```

## 習題

- [POI2011 R3 Day2 Meteors](https://loj.ac/p/2169)

[^1]: 每個邊只會往一邊走，上一層用完了就可以刪掉，所以同一時間只有 $m$ 條邊在跑，每個邊只出現在一個地方 