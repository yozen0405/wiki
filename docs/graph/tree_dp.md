<https://taodaling.github.io/blog/2019/09/10/%E6%A0%91%E4%B8%8A%E7%AE%97%E6%B3%95/#heading-%E6%A0%91%E4%B8%8A%E5%8C%B9%E9%85%8D%E9%97%AE%E9%A2%98>

???+note "[hackerrank kingdom division](https://www.hackerrank.com/challenges/kingdom-division/problem)"
	給一個 $n$ 個點的樹，每個點可以圖黑色或白色
	
	同一種顏色不一定要擠在同一個連通塊，但是每個同色的連通塊至少要有 $2$ 個點
	
	問有幾種圖法
	
	$n\le 10^5$
	
	??? note "思路"
		$f(u,0/1):$ $u$ 是白色/黑色的塗色方法數
		
		$g(u,0/1):$ $u$ 是白色/黑色，$v$ 都是跟 $u$ 相反的顏色的方法數
		
		$f(u,0)=g(u,0)\times(g(v,0)+f(v,0))+f(u,0)\times(f(v,0)+g(v,0)+f(v,1))$
	    $f(u,1)=g(u,1)\times(g(v,1)+f(v,1))+f(u,1)\times(f(v,1)+g(v,1)+f(v,0))$
	    
	    $g(u,0)=g(u,0)\times f(v,1)$
	    
	    $g(u,1)=g(u,1)\times f(v,0)$		
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    using namespace std;
	
	    const int INF = 0x3f3f3f3f;
	    const int M = 1e9 + 7;
	    const int maxn = 1e5 + 5;
	    int n, d;
	    vector<int> G[maxn];
	    int f[maxn][2], g[maxn][2];
	
	    void dfs (int u = 1, int par = 0) {
	        g[u][0] = g[u][1] = 1;
	        f[u][0] = 0, f[u][1] = 0;
	
	        for (auto v : G[u]) {
	            if (par == v) continue;
	
	            dfs(v, u);
	            f[u][0] = (g[u][0] * (g[v][0] + f[v][0])) % M + 
	                      (f[u][0] * (f[v][0] + g[v][0] + f[v][1])) % M;
	            f[u][0] %= M;
	
	            f[u][1] = (g[u][1] * (g[v][1] + f[v][1])) % M + 
	                      (f[u][1] * (f[v][1] + g[v][1] + f[v][0])) % M;
	            f[u][1] %= M;
	
	            g[u][0] *= f[v][1];
	            g[u][0] %= M;
	
	            g[u][1] *= f[v][0];
	            g[u][1] %= M;
	        }
	    }
	
	    signed main() {
	        cin >> n;
	
	        for (int i = 0; i < n - 1; i++) {
	            int u, v;
	            cin >> u >> v;
	            G[u].push_back(v);
	            G[v].push_back(u);
	        }
	
	        dfs ();
	        cout << (f[1][0] + f[1][1]) % M << "\n";
	    }
	    ```
???+note "[CF 461B Appleman and Tree](https://codeforces.com/problemset/problem/461/B)"
	給一棵 $n$ 個點的樹，每個點是白色或者黑色，問有多少種方案能夠通過去掉一些邊，使每個聯通塊中只有一個黑色的點
	
	$n\le 10^5$
	
	??? note "思路"
		$dp[u][0/1]:$ 當前 $u$ 所在的連通塊有 $0/1$ 個黑點的切邊方法數
		
		技巧 : 利用之前的狀態合併
		
		```cpp linenums="1"
		void dfs (int u, int par) {
	        if (a[u]) dp[u][1] = 1;
	        else dp[u][0] = 1;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs (v, u);
	
	            dp[u][1] = ((dp[u][1] * (dp[v][0] + dp[v][1])) % M 
	            			+ (dp[u][0] * dp[v][1]) % M) % M;
	            dp[u][0] = (dp[u][0] * (dp[v][0] + dp[v][1])) % M;
	        }
	    }
	    ```

???+note "[vijos 1892 树上的最大匹配](https://vijos.org/p/1892)"
	給 $n$ 個點的樹，問最多可以選幾條邊滿足都每個點只有最多一條與之相連的邊有選，輸出最多可以選幾條邊與方案數
	
	$n\le 1.5\times 10^6$

???+note "[CF 1856 E1. PermuTree (easy version)](https://codeforces.com/contest/1856/problem/E1)"
	給一顆 $n$ 個點的 Tree，問對於所有從 $1\ldots n$ 的 permutation $a$，最大的 $f(a)$ 是多少
	
	$f(a)=$ 有幾個 pair $(u,v)$ 滿足 $a_u < a_{\text{lca}(u,v)}<a_v$
	
	$2\le n\le 5000$
	
	??? note "思路"
		對於定根 $u$，我們會想讓下面每顆子樹的 $a_i$ 都恰好是連續的一段，因為這樣下面的點可以對 $u$ 造成貢獻，又可以對 $v$ 造成貢獻。然後因為是連續的一段，可以遞迴下去變子問題。
		
		接下來要考慮對於定根 $u$，有那些 $v$ 的 $a_i$ 會小於他，有那些 $v$ 的 $a_i$ 會大於他，然後就只要把小於他跟大於他的數量相乘即是對答案的貢獻。這邊就可以使用類似 01 背包的 dp，dp[w] 代表是否能取一些 v 使得 sum(dp[v]) = w，最後我們對每個有可能取到的 w 算 w * (sz[u] - w - 1) 的最大值，將 ans 加上他即可
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define F first
	    #define S second
	
	    using namespace std;
	
	    const int maxn = 5050;
	    vector<int> G[maxn];
	    int sz[maxn];
	    int ans = 0;
	
	    void dfs1(int u, int par) {
	        sz[u] = 1;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs1(v, u);
	            sz[u] += sz[v];
	        }
	    }
	
	    void dfs2(int u, int par) {
	        vector<pair<int, int>> sizes;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            sizes.push_back({sz[v], v});
	        }
	
	        for (auto x : sizes) {
	            dfs2(x.S, u);
	        }   
	
	        vector<int> dp (sz[u] + 1, false);
	        dp[0] = true;
	
	        for (int i = 0; i < (int)sizes.size(); i++) {
	            vector<int> newDp (sz[u] + 1, false);
	            for (int j = 0; j <= sz[u]; j++) {
	                // take
	                if (j >= sizes[i].F)
	                    newDp[j] |= dp[j - sizes[i].F];
	                // not take
	                newDp[j] |= dp[j];
	            }
	            swap(dp, newDp);
	        }
	
	        int mxAdd = 0;
	        for (int j = 0; j <= sz[u]; j++) {
	            if (dp[j])
	                mxAdd = max(mxAdd, j * (sz[u] - 1 - j));
	        }
	
	        ans += mxAdd;
	    }
	
	    void solve() {
	        int n;
	        cin >> n;
	        for (int i = 1; i < n; i++) {
	            int v;
	            cin >> v;
	            v--;
	            G[i].push_back(v);
	            G[v].push_back(i);
	        }
	
	        dfs1(0, -1);
	        dfs2(0, -1);
	        cout << ans << '\n';
	    }
	
	    signed main() {
	        solve();
	    }
		```

???+note "[CS Academy Experience](https://csacademy.com/contest/archive/task/experience/)"
	給一棵 $n$ 個點的樹，點有權重，你要把它切成一些 chain，使得每個 chain 的最大權重減最小權重的總和盡量大。
	
	$n \le 10^5$
	
	??? note "思路"
		對於 max - min 我們可以想成好幾個差值組合起來的，例如 2 → 5 → 7 → 8 可以寫成 8 - 2 = 8 - 7 - 5 - 2。
		
		所以對於最後的答案每個 chain 的兩端一定是 max 跟 min，而且變化會是持續遞減或持續遞增，如果不是，例如 6 → 5 → 4 → 5 → 2，那我們可以將 {6, 5, 4} 跟 {5, 2}，還能讓答案變更大。
	
		所以我們定義 dp[u][0/1]: u 這個點是 min/max 端，u 這顆子樹的答案
        
        轉移的話一種情況是 u 自己一組，= sum(max(dp[u][0], dp[u][1]))
        
        令一種情況是 u 有被接到 v 往下的 chain
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        using namespace std;
        #define int long long

        const int maxn = 1e5 + 5;
        int n;
        int w[maxn], dp[maxn][2];;
        vector<int> G[maxn];

        void update(int &x, int y) {
            if (x < y) x = y;
        }

        void dfs(int u, int pa) {
            int sum = 0;
            for (auto v : G[u]) {
                if (v == pa) continue;
                dfs(v, u);
                sum += max(dp[v][0], dp[v][1]);
            }
            dp[u][0] = dp[u][1] = sum;
            for (auto v : G[u]) {
                if (v == pa) continue;
                update(dp[u][0], sum + dp[v][0] + w[v] - w[u] - max(dp[v][0], dp[v][1]));
                update(dp[u][1], sum + dp[v][1] + w[u] - w[v] - max(dp[v][0], dp[v][1]));
            }
        }

        signed main() {
            cin >> n;
            for (int i = 1; i <= n; i++) {
                cin >> w[i];
            }
            for (int i = 1; i < n; i++) {
                int u, v;
                cin >> u >> v;
                G[u].push_back(v);
                G[v].push_back(u);
            }
            dfs(1, 1);
            cout << max(dp[1][0], dp[1][1]) << '\n';
        }
        ```
        
??? note "[CS Academy Growing Trees](https://csacademy.com/contest/archive/task/growing-trees)"
	給一棵 $n$ 個點的樹，在第 $k$ 天的時候第 $i$ 條邊的權重是 $C_i + k \times A_i$，求在第 $[0, D]$ 天內，什麼時候會有最短的樹直徑
	
	$n\le 2.5 \times 10^5,0\le D\le 10^6$
	
	??? note "思路"
		觀察到 $C_i + k \times A_i$ 會是一個峰函數，可以三分搜極值