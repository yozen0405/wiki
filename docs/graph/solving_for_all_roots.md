又稱全方位木 DP，solving for all roots

- <https://usaco.guide/gold/all-roots?lang=cpp>

???+note "[CSES Tree Distances I](https://cses.fi/problemset/task/1132/)"
	給一顆有 $n$ 個點的樹，對於每個點求該點到其他點的最遠距離
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		> 法 1 : 換根 dp
		
		因為在給 $v$ 計算的時候有可能 $u$ 的最遠距離就在 $v$ 子樹
		
		所以必須維護次遠距離的子樹
		
		??? code "實作"
			```cpp linenums="1"
			#include <bits/stdc++.h>
	        #define int long long
	        using namespace std;
	
	        const int maxn = 2e5 + 5;
	        const int INF = 0x3f3f3f3f;
	        int n;
	        int dp_ch[maxn], dp_fa[maxn], sec[maxn], ans[maxn];
	        vector<int> G[maxn];
	
	        void dfs_ch (int u, int par) {
	            for (auto v : G[u]) {
	                if (v == par) continue;
	
	                dfs_ch (v, u);
	
	                if (dp_ch[v] + 1 > dp_ch[u]) {
	                    sec[u] = dp_ch[u];
	                    dp_ch[u] = dp_ch[v] + 1;
	                }
	                else if (dp_ch[v] + 1 > sec[u]) {
	                    sec[u] = dp_ch[v] + 1;
	                }
	            }
	        }
	
	        void dfs_fa (int u, int par) {
	            for (auto v : G[u]) {
	                if (v == par) continue;
	
	                if (dp_ch[v] + 1 == dp_ch[u]) {
	                    dp_fa[v] = max(sec[u], dp_fa[u]) + 1;
	
	                    dfs_fa (v, u);
	                }
	                else {
	                    dp_fa[v] = max(dp_ch[u], dp_fa[u]) + 1;
	                    dfs_fa (v, u);
	                }
	            }
	        }
	
	        signed main() {
	            ios::sync_with_stdio(0);
	            cin.tie(0);
	            cin >> n;
	
	            for (int i = 0, u, v; i < n - 1; i++) {
	                cin >> u >> v;
	                G[u].push_back(v);
	                G[v].push_back(u);
	            }
	
	            dfs_ch (1, 0);
	            dfs_fa (1, 0);
	
	            for (int i = 1; i <= n; i++) {
	                cout << max (dp_ch[i], dp_fa[i]) << " ";
	            }
	        }
	        ```
	        
		> 法 2 : 樹直徑
		
		根據樹直徑的性質，最遠點一定會在樹直徑的頭尾
		
		證明請參見樹直徑的 section

???+note "<a href="/wiki/graph/images/403 . 樹直徑.html" target="_blank">2023 IOIC 403 .樹直徑</a>"
	有一棵 $N$ 個點的樹
	
	進行如下操作**一次**：砍掉樹上的其中一條邊，再另外加上一條邊回去使其保持連通。
	
	可以透過這個操作讓樹直徑最小為何？
	
	$N\le 5\times 10^5$
	
	??? note "code"
		```cpp linenums="1"
		void diameter_fa(int u, int par) {
	        for (int v : adj[u]) {
	            if (v == par) continue;
	            // 斷邊 (u, v)，u 以上含 u 的連通塊的答案
	
	            // 清除 v 在 ans(u) 的貢獻
	            erase_one(ch_dp[u], dp_ch[v]);
	            erase_one(ch_hei[u], hei[v] + 1);
	
	            // 計算答案 max (樹直徑, 往上或往下的最大高度+往上或往下的次小高度)
	            dp_fa[v] = mav(*rbegin(ch_dp[u]), *rbegin(ch_hei[u]) + *nevt(rbegin(ch_hei[u])));
	
	            ch_dp[v].insert(dp_fa[v]);
	            // 由 v -> u -> ... 往上的最大高度
	            ch_hei[v].insert(*rbegin(ch_hei[u]) + 1);
	
	            // 加回 v 在 ans(u) 的貢獻
	            ch_dp[u].insert(dp_ch[v]);
	            ch_hei[u].insert(hei[v] + 1);
	
	            diameter_fa(v, u);
	        }
	    }
		```

???+note "[CF 1029F Tree with Maximum Cost](https://codeforces.com/contest/1092/problem/F)"
	求 $\max \limits_{v=1... n} \{\space \sum\limits_{i = 1}^{n} dist(i, v) \cdot a_i \space \}$
	
	$n,a_i\le 2\times 10^5$
	
	??? note "思路"
		一樣換根 dp
	
		不過這邊在算 $fa$ 的時候我們採用直接算答案的方式比較方便
		
		也就是 $dp_{fa}[u]$ 直接紀錄 $u$ 的答案
		
		$$\small dp_{fa}[v] = (dp_{fa}[u] - \underbrace{(dp_{ch}[v] + sum[v] + a[v])}_{扣除v的貢獻} + \underbrace{(tot - sum[v] - a[v])}_{加上v子樹以外的貢獻}) + dp_{ch}[v]$$
		
	    不然還要再多紀錄 $u$ 上面的連通塊的 sum
	    
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
	    using PQ = priority_queue<int, vector<int>, greater<int>>;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    int n, tot;
	    int a[maxn];
	    int dp1[maxn], dp2[maxn], sum[maxn];
	    vector<int> G[maxn];
	
	    void dfs1 (int u, int par) {
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs1 (v, u);
	
	            sum[u] += sum[v] + a[v];
	            dp1[u] += dp1[v] + sum[v] + a[v];
	        } 
	    }
	
	    void dfs2 (int u, int par) {
	        for (auto v : G[u]) {
	            if (v == par) continue;
	
	            dp2[v] = (dp2[u] - (dp1[v] + sum[v]) - a[v] + tot - sum[v] - a[v]) + dp1[v];
	            dfs2 (v, u);
	        }
	    }
	
	    void init () {
	        cin >> n;
	        for (int i = 1; i <= n; i++) cin >> a[i], tot += a[i];
	        int u, v;
	
	        for (int i = 1; i <= n - 1; i++) {
	            cin >> u >> v;
	            G[u].pb (v);
	            G[v].pb (u);
	        }
	    }
	
	    void solve () {
	        dfs1 (1, 0);
	        dp2[1] = dp1[1];
	
	        dfs2 (1, 0);
	
	        int ans = -INF;
	        for (int i = 1; i <= n; i++) {
	            ans = max (ans, dp2[i]);
	        }
	
	        cout << ans << "\n";
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            init();
	            solve();
	        }
	    } 
	    ```


???+note "[Balkan OI 2017 City Attractions ](https://www.acmicpc.net/problem/14875)"
	給一 $N$ 個點的顆樹，有一個人第一天在 $1$ 這個節點
	
	每天他會從昨天停下來的點 $x$ 走到點 $y$
	
	$y$ 滿足 $y\neq x$ 且 $a_y-\text{dis}(x,y)$ 是最小的，若這樣還是有多個 $y$ 就選 index 最小的
	
	問第 $K$ 天會在哪個點停下
	
	$N\le 3\times 10^5,K\le 10^{18},a_i\le 10^9$
	
	??? note "思路"
		我們只要預處理好對於每個 $x$ 他會走到的 $y$ 就可以用倍增法找第 $K$ 天的結果
		
		所以我們來看要如何預處理
		
		直接換根 dp
		
		<figure markdown>
	      ![Image title](./images/17.png){ width="300" }
	    </figure>
	    
	    細節與實作可參考[這篇 USACO 博客](https://usaco.guide/problems/balkan-oi-2017city-attractions/solution)


???+note "[Atcoder Educational DP Contest V - Subtree](https://atcoder.jp/contests/dp/tasks/dp_v)"
	給你一個 $n$ 點的樹，問說在裡面的一些點圖黑色，其他點圖白色，且在圖黑色的點要連通，輸出有幾種圖法
	
	$n\le 10^5$
	
	??? note "思路"
		$dp(u,0/1):$ $u$ 是白/黑，黑色要能連通的能塗色的方法數
		
		對於所有的點， $dp_{ch}(u,0)=1,dp_{fa}(u,0)=1$
	
		$dp_{ch}(u,1)=\prod (dp_{ch}(v,0)+dp_{ch}(v,1))$
		
		$dp_{fa}(v_1,1)=dp_{fa}(u,1)\times \prod \limits_{v\neq v_1} (dp_{ch}(v,0)+dp_{ch}(v,1))+dp_{fa}(u,0)$
		
		$ans_u=dp_{ch}(u,1)\times dp_{fa}(u,1)$
		
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
	    using PQ = priority_queue<int, vector<int>, greater<int>>;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	
	    int n, M;
	    vector<int> G[maxn];
	    int dp_ch[maxn][2],dp_fa[maxn][2];
	
	    void dfs_ch (int u, int par) {
	        dp_ch[u][1] = dp_ch[u][0] = 1;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs_ch (v, u);
	
	            dp_ch[u][1] = dp_ch[u][1] * (dp_ch[v][0] + dp_ch[v][1]);
	            dp_ch[u][1] %= M;
	        }
	    }
	
	    void dfs_fa (int u, int par) {
	        dp_fa[u][0] = 1;
	        int sz = G[u].size ();
	
	        vector<int> pre (sz + 1, 1);
	        vector<int> suf (sz + 2, 1);
	
	        for (int i = 1; i <= sz; i++) {
	            int v = G[u][i - 1];
	            if (v == par) {
	                pre[i] = pre[i - 1];
	                continue;
	            }
	
	            pre[i] = (pre[i - 1] * (dp_ch[v][0] + dp_ch[v][1])) % M;
	        }
	        for (int i = sz; i >= 1; i--) {
	            int v = G[u][i - 1];
	            if (v == par) {
	                suf[i] = suf[i + 1];
	                continue;
	            }
	            suf[i] = (suf[i + 1] * (dp_ch[v][0] + dp_ch[v][1])) % M;
	        }
	
	        for (int i = 1; i <= sz; i++) {
	            int v = G[u][i - 1];
	            if (v == par) {
	                continue;
	            }
	
	            dp_fa[v][1] = (dp_fa[u][1] * ((pre[i - 1] * suf[i + 1]) % M)) % M + dp_fa[u][0];
	            dp_fa[v][1] %= M;
	        }
	
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs_fa (v, u);
	        }
	    }
	
	    void init () {
	        cin >> n >> M;
	
	        int u, v;
	        for (int i = 1; i <= n - 1; i++) {
	            cin >> u >> v;
	            G[u].pb (v);
	            G[v].pb (u);
	        }
	    }
	
	    void solve () {
	        dfs_ch (1, -1);
	
	        dp_fa[1][1] = 1;
	        dfs_fa (1, -1);
	
	        for (int i = 1; i <= n; i++) {
	            cout << (dp_ch[i][1] * dp_fa[i][1]) % M << "\n";
	        }
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            init();
	            solve();
	        }
	    } 
	    ```

???+note "[CF 708 C. Centroids](https://codeforces.com/problemset/problem/708/C)"
	給你一棵 $n$ 個點的樹，對於每個點，在可以修改一條邊的前提下，能否讓這個點成為樹重心
	
	$n\le 4\times 10^5$
	
	??? note "思路"
	
        以 u 為根的話，最多只會有一個 subtree 的 size > n/2

        所以我們進行換根 dp，分別考慮 subtree 在 v 下面(含 v)，與上面

        在下面的 case 只要看有沒有 v(含 v) 下面的 subtree 的 size ≤ n / 2，我們取最大的，叫做 max_sz[u]

        若有 sz[v] ≥ n / 2，看看有沒有符合 sz[v] - max_sz[v] ≤ n / 2

        max_sz[u]: u 下面 size ≤ n / 2 且最大的

        當 dfs(u → v) 的時候，上面的就有分兩種 case:

        - case1: 把連著 u 的整塊都拔掉
            - n - sz[v]

        - case2: 將除了 u 以外，u 以下的拔掉
            - n - sz[v] - max(pre[i - 1], suf[i + 1])

        最後就檢查是否上，下都合法即可

        > 參考 : [CSDN 題解](https://blog.csdn.net/Miracle_ma/article/details/52317440?spm=1001.2101.3001.6650.13&utm_medium=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13-52317440-blog-121302860.237%5Ev3%5Ewap_relevant_t0_download&depth_1-utm_source=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-13-52317440-blog-121302860.237%5Ev3%5Ewap_relevant_t0_download)
        
## 其餘的題單

- CF 1187 E. Tree Painting

- CF 1092 F. Tree with Maximum Cost

- CF 1324 F. Maximum White Subtree

- CF 219 D. Choosing Capital for Treeland

- CF 149 D. Coloring Brackets