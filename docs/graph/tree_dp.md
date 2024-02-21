???+note "[hackerrank kingdom division](https://www.hackerrank.com/challenges/kingdom-division/problem)"
	給一個 $n$ 個點的樹，每個點可以圖黑色或白色，同一種顏色不一定要擠在同一個連通塊，但是每個同色的連通塊至少要有 $2$ 個點。問有幾種圖法
	
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
	
	    void dfs(int u = 1, int par = 0) {
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
	
	        dfs();
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
		void dfs(int u, int par) {
	        if (a[u]) dp[u][1] = 1;
	        else dp[u][0] = 1;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs(v, u);
	
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
		考慮局部貪心，假設我們現在在節點 $u$，我們希望 $u$ 不同子樹中的 $(v,w),\ a_v < a_u < a_w$ 的對數盡量多。

		我們實際上只關心子樹內 $a_u$ 的相對大小關係，不關心它們具體是什麼。如果 $u$ 只有兩個兒子 $v$ 和 $w$，我們可以讓 $v$ 子樹內的 $a$ 全部小於 $w$ 子樹內的 $a$，這樣 $u$ 作為 $LCA$ 的貢獻是 $sz_v \times sz_w$，是最大的。備註: 在多個小孩的時候，他們每個子樹都是一個獨立的子問題，也就是以 $u$ 為根的話他們每個子樹內的 $a$ 的值域都是一個連續的區間，例如 $a_u=6$，$v_1$ 子樹內的 $a=1,2$，$v_2$ 子樹內的 $a=3,4,5$，$v_3=7,8$，不過這個對問題的影響不大，因為在 $u$ 看來只關心他們與 $a_u$ 的大小，並且每個子樹都是一個子問題。
		
		那麼對於節點 $u$ 有多個兒子的情況，推廣可知相當於把 $u$ 的兒子分成 $S$ 和 $T$ 兩個集合，最大化 $\sum_{v \in S} \sum_{w \in T} sz_v \times sz_w$。考慮做一個 $sz_v$ 的 01 背包，若能把 $sz_v$ 分成大小為 $x$ 的集合，$u$ 對答案的貢獻是 $x \times (sz_u - 1 - x)$。對於可能的 $x$，取 $x \times (sz_u - 1 - x)$ 的最大值即可。

		01 背包暴力做即可，所以時間複雜度 $O(n^2)$。
		
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
		其實就是 <a href="/wiki/dp/problem/#cf-484d" target="_blank">CF 484D</a> 的樹上版本
	
		對於 max - min 我們可以想成好幾個差值組合起來的，例如 2 → 5 → 7 → 8 可以寫成 8 - 2 = 8 - 7 - 5 - 2。
		
		所以對於最後的答案每個 chain 的兩端一定是 max 跟 min，這個已在上面 CF 484D 的題解裡面提到了。
	
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

???+note "[CS Academy Connected Tree Subgraphs](https://csacademy.com/contest/archive/task/connected-tree-subgraphs)"
	給一個 $n$ 個點的 Tree，有幾種用 1~n 去編號的方法滿足 :
	
	- 對於 $1\le k\le n$，由編號 $1\ldots k$ 組成的子圖要連通
	
	$1\le n\le 10^5$
	
	??? note "思路"
		觀察到他是起點慢慢長出去的，所以我們先考慮將點 1 定根，上面放編號 1
		
		對於 $u$，我們假設我們能算出 $dp_v$，那麼 $dp_u$ 要怎麼計算呢 ?
		
		我們假設放的順序形成一個序列 $p$，對於同一個子樹我們不必困擾放的順序，因為 $dp_v$ 已計算完成，所以我們可以將同一個 subtree$(v)$ 裡面的順序視為同物，也就是 $p$ 就會是好幾個同物排列。
		
		所以 $dp_u=$ (同物排列的方法數) $\times \prod dp_v$ $\displaystyle =\frac{(size(u)-1)!}{\prod size(v)!}\times \prod dp_v$ 

???+note "[CF 1929 D. Sasha and a Walk in the City](https://codeforces.com/problemset/problem/1929/D)"
	給一顆 $n$ 個點的樹，定義一個合法的點集為任意一條路徑都沒有超過兩個在這個點集內的點。問對於所有可能的點集，有幾種是合法的
	
	$2\le n\le 3\times 10^5$
	
	??? note "思路"
		問方法數，我們自然而然會想到樹 dp。假設一個 v 為根子樹內的合法方案數是 dp(v)，我們考慮如何轉移上去 v 的 parent u。我們發現我們無法區分子樹內選得點有祖孫關係還是沒有祖孫關係，因為這會關係到我們 u 如果一定要選的合法方案數，這會造成 dp(v) 內有祖孫關係直接是不合法的，而沒有祖孫關係的合法。
		
		所以我們需要區分這兩種狀態，令：
		
		- dp(i, 0): 以 i 為根的子樹有選的點「不存在祖孫關係」的合法方法數
	
		- dp(i, 1): 以 i 為根的子樹有選的點「恰有祖孫關係」的合法方法數
	
		考慮 v 與他的父親 u 的轉移，dp(u, 0) 由於不能有祖孫關係，所以當 u 點要選時，下面的點都不能選，為 1 種方法數，如果不選 u，則就是下面的每顆子樹的 dp(v, 0) 相乘起來。dp(u, 1) 由於一定要有祖孫關係，但又要合法，所以當 u 選的時候，下面只能挑一顆沒有祖孫關係的子樹，也就是將每顆子樹 dp(v, 0) 扣 1 樹加起來，扣 1 的原因是要扣除什麼都沒挑的情況，當 u 不選的時候，就是枚舉哪顆子樹要有祖孫關係，其他的子樹什麼都不能選，也就是 dp(v, 1) 加起來。我們統整一下，轉移式為： 
		
	    - $dp(u, 0) = 1 + \prod dp(v, 0)$
	
	    - $dp(u, 1) = \sum(dp(v, 0) - 1) + \sum dp(v, 1)$
	
	    最後的答案就是根節點的 「沒有祖孫關係的方法數」 + 「有祖孫關係的方法數」，也就是 dp(root, 0) +  dp(root, 1)。
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    using namespace std;
	
	    const int MAXN = 2e6 + 5;
	    const int mod = 998244353;
	    vector<int> G[MAXN];
	    int dp[MAXN][2];
	
	    void dfs(int u, int lst) {
	        dp[u][0] = 1; // leaf 有 1 種選法叫做什麼都不選
	        dp[u][1] = 0;
	        for (auto v : G[u]) {
	            if (v == lst) continue;
	            dfs(v, u);
	            dp[u][0] = (dp[v][0]) * dp[u][0] % mod;
	            dp[u][1] = (dp[v][0] - 1 + dp[v][1]+ dp[u][1]) % mod;
	        }
	        dp[u][0]++; // 只選 u
	    }
	
	    void solve() {
	        int n;
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            G[i].clear();
	        }
	        for (int i = 0; i < n - 1; i++) {
	            int u, v;
	            cin >> u >> v;
	            G[u].push_back(v);
	            G[v].push_back(u);
	        }
	
	        dfs(1, 0);
	
	        cout << (dp[1][0] + dp[1][1]) % mod << endl;
	    }
	
	    signed main() {
	        int t = 1;
	        cin >> t;
	        while (t--) {
	            solve();
	        }
	    }
	    ```

???+note "[POI 2017 Sabotaż](https://www.luogu.com.cn/problem/P5958)"
	給一顆 n 個點的有根樹，其中有 1 個點是叛徒，但不知道是誰。對於一個點 u，若 subtree(u) 中叛徒佔的比例超過 x，那 u 也會變成叛徒，且 subtree(u) 內所有點都變叛徒。求出一個最小的 x，使得最壞情況下，叛徒的個數不會超過 k。
	
	$k\le n\le 5\times 10^5$
	
	??? note "思路"
		第一個想法是二分，若目前 threshold 為 x，我們就是要去 check 是否有辦法讓最壞情況下，叛徒的個數 <= k。我們假設 dp(u) 表示最壞情況下 u 的子樹裡有幾個叛徒，轉移的話我們就去枚舉 u 的小孩 v，看最一開始的那個叛徒要在哪個子樹內才會是最糟糕的情況，也就是看哪個 dp(v) 最大。如果 dp(v) 的比例有到 x 的話就將 dp(u) = size(u)，否則 dp(u) = max{ dp(v) }。這個想法雖然是 O(n log n)，但會 TLE。
		
		第二個想法是樹 dp，設 dp(u) 表示節點 u 不叛變，x 最小可以是多小。因為精度問題，dp(u) 也表示節點 u 叛變，x 最大可以是多大。若要使節點 u 能叛變，我們必須得滿足兩個條件：
	
	    1. 它有至少一棵子樹叛變，即 $x \le dp(v)$。
	    2. $x\le \dfrac{sz(v)}{sz(u) - 1}$。
	
		既然我們想找出讓 u 叛變最小的 x，對上述兩者情況取 min 即可。再來，由於我們要的是在最壞情況下的答案（例如若第一個叛徒在某個子樹需要花的 x 很大），所以我們要找最大值，那麼狀態轉移方程式如下：
		
		$$dp(u) = \max \limits_{v \in son(u)}\{\min\{ dp(v), \frac{sz(v)}{sz(u) - 1} \} \}$$
		
	??? note "思路1 - code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	
	    const int N = 5e5 + 5;
	    const double eps = 1e-6;
	
	    vector<int> G[N];
	    int dp[N], sz[N];
	    int n, k;
	
	    void tree_init(int u) {
	        sz[u] = 1;
	        for (int i = 0; i < (int)G[u].size(); i++) {
	            int v = G[u][i];
	            tree_init(v);
	            sz[u] += sz[v];
	        }
	    }
	
	    void tree_dp(int u, double x) {
	        if (G[u].empty()) {
	            dp[u] = 1;
	            return;
	        }
	        int mx = 0;
	        for (int i = 0; i < (int)G[u].size(); i++) {
	            int v = G[u][i];
	            tree_dp(v, x);
	            mx = max(mx, dp[v]);
	        }
	        if (((double)mx / (double)(sz[u] - 1)) > x) {
	            dp[u] = sz[u];
	        } else {
	            dp[u] = mx;
	        }
	    }
	
	    bool check(double x) {
	        memset(dp, 0, sizeof dp);
	        tree_dp(1, x);
	        return dp[1] <= k;
	    }
	
	    signed main() {
	        ios::sync_with_stdio(0);
	        cin.tie(0);
	        cin >> n >> k;
	        for (int i = 2; i <= n; i++) {
	            int v;
	            cin >> v;
	            G[v].push_back(i);
	        }
	        tree_init(1);
	        double l = 0, r = 1;
	        while (r - l > eps) {
	            double mid = (l + r) / 2;
	            if (check(mid)) {
	                r = mid;
	            } else {
	                l = mid;
	            }
	        }
	        cout << fixed << setprecision(10) << r << '\n';
	    }
	    ```
	    
	??? note "思路2 - code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	
	    int n, k, head[500005], tot, siz[500005];
	    double ans, dp[500005];
	
	    struct edge {
	        int next, to;
	    } e[1000005];
	
	    void add_edge(int u, int v) {
	        e[++tot].next = head[u];
	        e[tot].to = v;
	        head[u] = tot;
	    }
	
	    void dfs(int u, int fa) {
	        siz[u] = 1;
	        for (int i = head[u]; i; i = e[i].next) {
	            dfs(e[i].to, u);
	            siz[u] += siz[e[i].to];
	        }  // 预处理size
	        if (siz[u] == 1) {
	            dp[u] = 1;
	            return;
	        }  // 初始值
	        for (int i = head[u]; i; i = e[i].next) {
	            int v = e[i].to;
	            dp[u] = max(dp[u], min(dp[v], 1.0 * siz[v] / (siz[u] - 1)));  // 重点
	        }
	        if (siz[u] > k)
	            ans = max(ans, dp[u]);  // 更新答案
	    }
	    int main() {
	        scanf("%d%d", &n, &k);
	        for (int i = 2; i <= n; i++) {
	            int x;
	            scanf("%d", &x);
	            add_edge(x, i);
	        }
	        dfs(1, 0);
	        printf("%.10lf", ans);
	    }
	    ```
---

## 參考資料

- <https://taodaling.github.io/blog/2019/09/10/%E6%A0%91%E4%B8%8A%E7%AE%97%E6%B3%95/#heading-%E6%A0%91%E4%B8%8A%E5%8C%B9%E9%85%8D%E9%97%AE%E9%A2%98>