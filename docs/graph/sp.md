## dijkstra

:arrow_forward: 單源點最短路徑

考慮不帶權的單點源最短路，我們用BFS維護一個 queue，每次處理一個點時最短路徑大小已知，因此只需要拿該點去**更新其他點一次**在**沒有負邊**的假設下，dijkstra 就像是有帶權的BFS。

???+note "模板 [CSES - Shortest Routes I](https://cses.fi/problemset/task/1671)"
	給一張帶正權無向圖,求從節點 $1$ 到其他所有節點的最短路徑。
	
	$1 \le n \le 10^5,1 \le m \le 2 \times 10^5$

一旦被選中去 relax 其他人時，就代表 u 這個點已經固定

??? note "算法實作1（稍慢）"
    ```cpp linenums="1"
    vector<int> dijkstra(int start) {
        vector<int> dis(n + 1, INF);
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, start});
        while (pq.size()) {
            auto [dis_u, u] = pq.top();
            pq.pop();
            if (dis[u] != INF) continue;
            dis[u] = dis_u;
            for (auto [v, w] : G[u]) {
                pq.push({w + dis[u], v});
            }
        }
        return dis;
    }
    ```
    
??? note "算法實作2（稍快）"
    ```cpp linenums="1"
    vector<int> dijkstra(int start) {
        vector<int> dis(n + 1, INF);
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, start});
        dis[start] = 0;
        while (pq.size()) {
            auto [dis_u, u] = pq.top();
            pq.pop();
            if (dis[u] < dis_u) continue; // 相當於 if (visited)
            dis[u] = dis_u;
            for (auto [v, w] : G[u]) {
                if (dis[v] > dis[u] + w) {
                    dis[v] = dis[u] + w;
                    pq.push ({dis[v], v});
                } 
            }
        }
        return dis;
    }
    ```

!!! info "補充 : O(n) 做 dijkstra"
    當圖邊權範圍上界在 $\approx 10^5$ 的時候，且權值具有單調性，可使用這個技巧

    實作一個 data structure，滿足以下功能 :
    
    - push(x)
    
    - get_value() 得到當前最小的 distance (相當於 pq.top())
    
    因為 distance 具有單調性，故 threshold 只會遞增。類似的技巧也應用在 [TIOJ 1915](https://tioj.ck.tp.edu.tw/problems/1915), [2023 一模 pD](/wiki/graph/wiki/graph/mst/#incremental)

??? note "為何 dijkstra 不能在有負邊的圖上跑 ?"
	簡單來說就是因為單調性。
	
	<figure markdown>
	  ![Image title](./images/141.png){ width="400" }
	</figure>
	
	舉例來說，現在要算出 A 到 D 的最短路徑，Dijkstra 首先 relax B, C，B 最短，然後把 B 的出邊進行鬆弛，B 被標記為處理過，然後再次選出 C，對 C 的出邊進行 relax，此時 B 雖然進行了鬆弛，但是前面已經被標記處理過了，所以最後算出來的最短路徑為 35，但是實際上最短路徑為 33。
	
	因為 Dijkstra 是這樣假設的：對於處理過的結點，{==沒有前往該結點的更短路徑==}，這種假設僅僅在{==沒有負權邊時==}才能成立。

### 練習

???+note "來回 [zerojudge g733. 110北二4.漫遊高譚市](https://zerojudge.tw/ShowProblem?problemid=g733)"
	給 $n$ 點 $m$ 邊有向圖，邊帶權，另外額外有 $k$ 條無向帶權邊，至多只能走一條這種邊。問 $s\to t$ 的最短路
	
	$n\le 10^4,m+k\le 10^5$
	
	??? note "思路"
		對於每個點 $u$ 找 $dis(s \rightarrow u) + dis(u \rightarrow v)$。正反各做一次，也就是把正圖跟反圖都各做一次 dijkstra。

???+note "[2021 南一中校內複賽 pC 為美好的地牢獻上爆擊](https://toj.tfcis.org/oj/pro/636/)"
	給一個 $n \times m$ 的棋盤，在某一個格子有一個 ADD 道具，其他每個格子都有一隻魔物攻擊力是 $w_{i,j}$。你要從左上角走到右下角，如果經過的格子有魔物，那你會受到 $w_{i,j}$ 點的傷害，並把那隻魔物打倒，第二次經過這個格子就不會再遇到魔物了。在經過 ADD 道具之後，每次你受到的傷害都會減少(但不會回血)，求你最少要承受多少傷害
	
	$n,m\le 10^3$
	
	??? note "思路"
		可分為不吃 ADD 與吃 ADD
		
		吃 ADD 的話 起點 &rarr; ADD &rarr; 終點 做兩張圖 dijkstra 即可

???+note "n 平方"
	給定 $n$ 個點，第 $i$ 點在 $(x_i,y_i)$，從 $i\to j$ 花費 $(x_i - x_j)^2 + (y_i - y_j)^2$。問從 $s\to t$ 的最小花費

???+note "字典序 [TIOJ 1572.最短路線問題(Path)](https://tioj.ck.tp.edu.tw/problems/1572)"
	給一張 $n$ 點 $m$ 邊無向圖，邊權皆為 $1$，輸出 $s\to t$ 的字典序最小的最短路徑
	
	$n,m\le 10^6$
	
	??? note "思路"
		從終點做回來，建立最短路徑 DAG，將 DAG 的邊反向，從起點每次 greedy 走最小的點直到抵達終點

???+note "最短平均路 [CF EDU B. Minimum Average Path](https://codeforces.com/edu/course/2/lesson/6/4/practice/contest/285069/problem/B)"
	給定一個 $n$ 點 $m$ 邊的 DAG，邊帶權，問從 $1\to n$ 的 path 上權重的平均最小可以是多少
	
	$n\le 10^5,m\le 10^5$
	
	??? note "思路"
		跟上面最大平均區間一樣，我們去二分搜平均值 $x$，將邊權都 $-x$，看有沒有一條 path 的總和 $\le 0$。我們可以令 $dp[i]=$ 走到點 $i$ 的最小權值是多少，$dp[v]=\min \{dp[u]+w \}$ 
		
	??? note "code"
		```cpp 
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	    #define pb push_back
	    #define mk make_pair
	    #define F first
	    #define S second
	    #define ALL(x) x.begin(), x.end()
	
	    using namespace std;
	
	    const double INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    int n, m;
	    double dp[maxn];
	    int par[maxn];
	    vector<pii> G[maxn];
	
	    bool check(double x) {
	        fill(dp, dp + n + 1, INF);
	        fill(par, par + n + 1, -1);
	        dp[1] = 0;
	        for (int i = 1; i <= n; i++) {
	            for (auto [v, w] : G[i]) {
	                double val = (double)dp[i] + w - x;
	                if (dp[v] > val) {
	                    dp[v] = val;
	                    par[v] = i;
	                }
	            }
	        }
	        return dp[n] <= 0;
	    }
	
	    signed main() {
	        cin >> n >> m;
	        for (int i = 1; i <= m; i++) {
	            int u, v, w;
	            cin >> u >> v >> w;
	            G[u].pb({v, w});
	        }
	
	        double l = 0, r = 105;
	        for (int i = 0; i < 100; i++) {
	            double mid = (l + r) / 2;
	            if (check(mid)) r = mid;
	            else l = mid;
	        }
	        check(l);
	
	        stack<int> stk;
	        int x = n;
	        stk.push(x);
	        while (par[x] != -1) {
	            x = par[x];
	            stk.push(x);
	        }
	        cout << stk.size() - 1 << '\n';
	        while (stk.size()) {
	            cout << stk.top() << ' ';
	            stk.pop();
	        }
	    } 
	    ```

### 多源點 dijkstra

???+note "[zerojudge b904. 10. 學園生活](https://zerojudge.tw/ShowProblem?problemid=b904)"
	給一張 $n$ 點 $m$ 邊的無向圖，與 $k$ 個源點，求這些點兩兩之間的距離最小值
	
	$n,k,m\le 3\times 10^5$
	
	??? note "思路"
		最暴力的想法就是枚舉源點，每次都重跑 dijkstra，複雜度 $O(k E \log⁡E )$
	
	    從上面暴力的方法我們可以觀察出，要交會的點或邊一定要是源點們之間的最短路。
	
	    每個源點能擴展出他能控制的最短路徑區域，如下圖
	
	    <figure markdown>
	      ![Image title](./images/5.png){ width="300" }
	      <figcaption>每個源點擴出自己的範圍</figcaption>
	    </figure>
	
	    範圍重疊的地方代表他們同時是多個源點的最短路徑，這個就是我們可以取的答案，我們可以用一個數字 $x$ 來控制每個範圍最多能擴張多少權重，一旦目前的 $x$ 能使某些個範圍重疊的這個 $x$ 就可以是答案，我們二分搜 $x$ 找到最小的 $x$ 使得範圍有重疊
	
	    二分搜的複雜度為 $O(\log ⁡C)$ 其中 $C$ 是值域範圍，每次都需要重新擴張源點的範圍(因為擴張的權重上限被更新了)，為 $O(E \log ⁡E )$ 所以複雜度 $O(E \log⁡ E \log⁡ C )$
	
	    但我們真的有需要每次都重新算嗎?
	
	    這邊提一個結構叫 shortest path tree 又稱最短路徑樹，每個點 $v$ 都跟自己的最短路徑的上一個點 $u$ 連接，形成一顆樹
	
	    我們可以建立最短路徑樹，我們就只需要在樹上 BFS 即可應付每次 $x$ 改變之後的擴張範圍。複雜度 $O(E\log⁡E)$ 建樹，$O(V+E)$ BFS 二分搜 $O(\log ⁡C)$，總共 $O(E \log ⁡E+(V+E)  \log ⁡C)$
	
	    但其實到頭來我們只是要看重疊的部分，我們也就同樣的建立最短路徑樹，枚舉 edge 使得 $(u,v)$ 是來自不同的源點，$ans$ 去跟他取 min 即可，複雜度 $O(E \log⁡ E+E) = O(E \log⁡ E)$
	
	    <figure markdown>
	      ![Image title](./images/6.png){ width="300" }
	      <figcaption>枚舉重疊邊</figcaption>
	    </figure>
		
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
	        const double EPS = 1e-8;
	
	        struct Graph {
	            vector<vector<pii>> G;
	            vector<int> source;
	            vector<int> dis;
	            vector<int> par;
	            int n;
	
	            void init(int _n) {
	                n = _n;
	                G.resize(n);
	            }
	
	            void add_edge(int u, int v, int w) {
	                G[u].pb({v, w});
	            }
	
	            void add_source(int x) {
	                source.pb(x);
	            }
	
	            void dijkstra() {
	                dis = vector<int>(n, INF);
	                par = vector<int>(n);
	                priority_queue<pii, vector<pii>, greater<pii>> pq;
	                for (auto s : source) {
	                    pq.push({0, s});
	                    dis[s] = 0;
	                    par[s] = s;
	                }
	
	                while (pq.size()) {
	                    auto [dis_u, u] = pq.top();
	                    pq.pop();
	
	                    if (dis_u > dis[u]) continue;
	                    dis[u] = dis_u;
	
	                    for (auto [v, w] : G[u]) {
	                        if (dis[u] + w < dis[v]) {
	                            dis[v] = dis[u] + w;
	                            par[v] = par[u];
	                            pq.push({dis[v], v});
	                        }
	                    }
	                }
	            }
	
	            bool check(int D) {
	                for (int i = 0; i < n; i++) {
	                    for (auto [v, w] : G[i]) {
	                        if (par[i] == par[v]) continue;
	                        int x = D - dis[i] - dis[v] - w;
	                        if (x >= 0) {
	                            return true;
	                        }
	                    }
	                }
	                return false;
	            }
	        } g;
	
	        int n, m, k;
	
	        void init() {
	            cin >> n >> m >> k;
	            g.init(n);
	            for (int i = 0; i < m; i++) {
	                int u, v, w;
	                cin >> u >> v >> w;
	                u--, v--;
	                g.add_edge(u, v, w);
	                g.add_edge(v, u, w);
	            }
	            for (int i = 0; i < k; i++) {
	                int u;
	                cin >> u;
	                u--;
	                g.add_source(u);
	            }
	        }
	
	        void work() {
	            g.dijkstra();
	            int l = 0, r = INF;
	            while (l < r) {
	                int mid = (l + r) / 2;
	                if (g.check(mid)) {
	                    r = mid;
	                } else {
	                    l = mid + 1;
	                }
	            }
	            cout << l << "\n";
	        }
	
	        signed main() {
	            // ios::sync_with_stdio(0);
	            // cin.tie(0);
	            int t = 1;
	            // cin >> t;
	            while (t--) {
	                init();
	                work();
	            }
	        }
			```
		
	    ---
	    
	    > 另解
	
	    對於每個非源點的點都去維護他與最近的兩個「不同的」源點的距離
	
	    令 $f[u]$ 為 $u$ 的與她最近源點的距離，$g[u]$ 為次近源點的距離，那麼答案就是 $ans =\min⁡(ans,f[u]+g[u])$
	    
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
	        const double EPS = 1e-8;
	
	        struct Graph {
	            vector<vector<pii>> G;
	            vector<int> source;
	            vector<int> f;
	            vector<int> parf;
	            vector<int> g;
	            vector<int> parg;
	            int n;
	
	            void init(int _n) {
	                n = _n;
	                G.resize(n);
	            }
	
	            void add_edge(int u, int v, int w) {
	                G[u].pb({v, w});
	            }
	
	            void add_source(int x) {
	                source.pb(x);
	            }
	
	            void dijkstra() {
	                f = vector<int>(n, INF);
	                parf = vector<int>(n);
	                priority_queue<pii, vector<pii>, greater<pii>> pq;
	                for (auto s : source) {
	                    pq.push({0, s});
	                    f[s] = 0;
	                    parf[s] = s;
	                }
	
	                while (pq.size()) {
	                    auto [dis_u, u] = pq.top();
	                    pq.pop();
	
	                    if (dis_u > f[u]) continue;
	                    f[u] = dis_u;
	
	                    for (auto [v, w] : G[u]) {
	                        if (f[u] + w < f[v]) {
	                            f[v] = f[u] + w;
	                            parf[v] = parf[u];
	                            pq.push({f[v], v});
	                        }
	                    }
	                }
	            }
	
	            int dijkstra2() {
	                g = vector<int>(n, INF);
	                parg = vector<int>(n, -1);
	                priority_queue<pii, vector<pii>, greater<pii>> pq;
	
	                for (int i = 0; i < n; i++) {
	                    for (auto [v, w] : G[i]) {
	                        if (parf[i] == parf[v]) continue;
	                        if (f[i] + w < g[v]) {
	                            g[v] = f[i] + w;
	                            parg[v] = parf[i];
	                            pq.push({g[v], v});
	                        }
	                    }
	                }
	
	                while (pq.size()) {
	                    auto [dis_u, u] = pq.top();
	                    pq.pop();
	
	                    if (dis_u > g[u]) continue;
	                    g[u] = dis_u;
	
	                    for (auto [v, w] : G[u]) {
	                        if (parg[u] == parf[v]) continue;
	                        if (g[u] + w < g[v]) {
	                            g[v] = g[u] + w;
	                            parg[v] = parg[u];
	                            pq.push({g[v], v});
	                        }
	                    }
	                }
	
	                int ans = INF;
	                for (int i = 0; i < n; i++) {
	                    if (parg[i] == -1) continue;
	                    if (parf[i] == parg[i]) continue;
	                    ans = min(ans, f[i] + g[i]);
	                }
	
	                return ans;
	            }
	        } g;
	
	        int n, m, k;
	
	        void init() {
	            cin >> n >> m >> k;
	            g.init(n);
	
	            for (int i = 0; i < m; i++) {
	                int u, v, w;
	                cin >> u >> v >> w;
	                u--, v--;
	                g.add_edge(u, v, w);
	                g.add_edge(v, u, w);
	            }
	
	            for (int i = 0; i < k; i++) {
	                int u;
	                cin >> u;
	                u--;
	                g.add_source(u);
	            }
	        }
	
	        void work() {
	            g.dijkstra();
	            cout << g.dijkstra2() << "\n";
	        }
	
	        signed main() {
	            // ios::sync_with_stdio(0);
	            // cin.tie(0);
	            int t = 1;
	            // cin >> t;
	            while (t--) {
	                init();
	                work();
	            }
	        }
	        ```


### shortest path tree

<figure markdown>
  ![Image title](./images/7.png){ width="250" }
  <figcaption>shortest path tree 結構</figcaption>
</figure>

??? note "實作"
	```cpp linenums="1"
    void build_Tree () {
        fill (par + 1, par + 1 + n, -1);
        for (int i = 1; i <= n; i++) {
            for (auto [v, w] : G[u]) {
                if (dis[v] == dis[u] + w) {
                    par[v] = u;
                }
            }
        }
        for (int i = 1; i < n; i++) {
            if (par[i] != -1) {
                D[par[i]].push_back(i);
            }
        }
    }
    ```

這邊帶一個相關的題目

???+note "[LOJ #3255. 「JOI 2020 Final」奥运公交](https://loj.ac/p/3255)"
    给 $n$ 點 $m$ 邊的有向圖，每條邊 $(u_i,v_i)$ 有邊權 $c_i$。你最多可以翻轉一條邊，翻轉代價是該條邊的 $d_i$，問從 $1$ 走到 $n$ 再走回 $1$ 的最小 cost
    
    $n \leq 200,m \leq 5 \times 10^4$
    
    ??? note "思路"
    	- 若翻轉的邊在 shortest path tree 上
    		- 因為邊被刪掉了，整顆 tree 就要重算
    		- $1 \to u \to v \to n$
    		- 你可以從 $1 \to u$ 但這之後要走哪一條? (已經沒有 $u \to v$ 了)
    
    	- 若翻轉的邊不在 shortest path tree 上
    		- 不經過邊: 原來的答案
    		- 要經過邊 $1 \to v \to u \to n \to 1$ 或 $1 \to n \to v \to u \to 1$
    
    	- 我們只需要邊在 tree 上時再從新算一次 dijkstra
    		- $O(n^3)$
    		- $O(n)$ 樹上最多 $n - 1$ 條邊
    		- $O(n^2)$ 暴力 dijkstra


### shortest path DAG

<figure markdown>
  ![Image title](./images/8.png){ width="300" }
  <figcaption>shortest path DAG結構</figcaption>
</figure>

有的都建邊，有重邊時也可以使用，時常配合 DAG DP 計算方法數

??? note "實作"
	```cpp linenums="1"
	void build_dag() {
        for (int u = 1; u <= n; u++) {
            for (auto [v, w] : G[u]) {
                if (dis[v] == dis[u] + w) {
                    D[u].push_back(v);
                    in[v]++;
                }
            }
        }
    }
    ```
	
???+note "[LOJ #2350. 「JOI 2018 Final」月票购买](https://loj.ac/p/2350)"
	給一張 $n$ 點 $m$ 邊帶權無圖，從 $s$ 到 $t$ 選一條**最短路徑**，將其邊權都設為 $0$。問 $u$ 到 $v$ 的最短路徑最小可以是多少
	
	$n\le 10^5,m\le 2\times 10^5$
	
	??? note "思路"
		- 若有重疊，設重疊為 $x\to \ldots \to y$
			- $ans=dis(u,x)+dis(y,v) \texttt{ or } dis(u,y)+dis(x,v)$
			- 記得跟 $dis(u,v)$ 取 min
	
		- 用 shortest path DAG 上枚舉 $x$ 用 dp 得到 $y$
			- DFS on DAG
			- $f[x]=$ topo sort 在 $x$ 之後的點的 $dis(v,y)$ 最小的 $y$
	
		- 同理用 shortest path DAG 找 $g[x]=$topo sort 在 $x$ 之後的點 $dis(u,y)$ 最小的 $y$
	
		- $ans=\min(dis(u,x)+f[x],dis(v,x)+g[x],dis(u,v))$  
	
	??? note "code"
		```cpp linenums="1"
		void dfs(int u) {
	        if (vis[u]) return;
	        vis[u] = 1;
	        f[u] = dV[u], g[u] = dU[u];
	        for (int i = head[u]; i; i = edge[i].next) {
	            int v = edge[i].to;
	            if (dS[u] + dT[v] + edge[i].len > dS[T]) continue;
	            dfs(v);
	            f[u] = min(f[u], f[v]), g[u] = min(g[u], g[v]);
	        }
	        ans = min({ans, f[u] + dU[u], g[u] + dV[u]});
	    }
	    ```

???+note "[LOJ #2344. 「JOI 2016 Final」铁路票价](https://loj.ac/p/2344)"
    給你一個 $n$ 點 $m$ 邊的無向圖，一開始每個邊的邊權都是 $1$，有 $q$ 個操作
    
    - $\text{change}(i,2):$ 將第 $i$ 條邊邊權變成 $2$
    
    每次操作完問有那些點跟原點的最短路不同
    
    $n\le 10^5,q,m\le 2\times 10^5$
    
    ??? note "有用的測資"
    	=== "input"
    	
            ```
            4 4 2
            1 2 
            2 3
            1 4
            4 3
            2
            1
            ```
        
        === "output"
        
            ```
            0
            2
            ```
    
    ??? note "思路"
    	一樣先建立 shortest path DAG，刪邊時類似 topo sort。假如現在刪掉 $(u,v)$ 這條邊，若 $in_v=0$ 就可以將變大傳遞給 $v$ 後面的點，由於每條邊邊權增加後就不可能出現在shortest path DAG，所以每條邊只需刪除一次故複雜度 $O(n+m)$。
    
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
    
        int ans;
    
        struct Edge {
            int u, v, w, id;
        };
    
        struct Graph {
            int n, m, s;
            vector<vector<Edge>> G;
            vector<vector<Edge>> D;
            vector<Edge> edges;
            vector<int> dis;
            vector<int> on_DAG;
            vector<int> in;
    
            Graph(int _n, int _m) {
                n = _n, m = _m;
                dis = vector<int>(n, INF);
                on_DAG = vector<int>(m);
                in = vector<int>(n);
                G.resize(n);
                D.resize(n);
            }
    
            void add_edge(int u, int v, int id) {
                int w = 1;
                G[u].pb({u, v, w, id});
                G[v].pb({v, u, w, id});
                edges.pb({u, v, w, id});
            }
    
            void dijkstra() {
                priority_queue<pii, vector<pii>, greater<pii>> pq;
                pq.push({0, s});
    
                while (pq.size()) {
                    auto [x, u] = pq.top();
                    pq.pop();
    
                    if (dis[u] != INF) continue;
                    dis[u] = x;
    
                    for (auto [u, v, w, id] : G[u]) {
                        pq.push({w + dis[u], v});
                    }
                }
            }
    
            void build_DAG() {
                for (int i = 0; i < n; i++) {
                    for (auto [u, v, w, id] : G[i]) {
                        if (dis[u] + w == dis[v]) {
                            D[u].pb({u, v, w, id});
                            on_DAG[id] = true;
                            edges[id] = {u, v, w, id};  // 要更新 edges 的方向
                            in[v]++;
                        }
                    }
                }
            }
    
            void del_edge(int eid) {
                if (on_DAG[eid] == false) return;
    
                queue<int> q;
                in[edges[eid].v]--;
                on_DAG[eid] = false;
                if (in[edges[eid].v] == 0) q.push(edges[eid].v);
    
                while (q.size()) {
                    int u = q.front();
                    q.pop();
                    ans++;
    
                    for (auto [u, v, w, id] : D[u]) {
                        if (on_DAG[id] == false) continue;
                        in[v]--;
                        on_DAG[id] = false;
                        if (in[v] == 0) {
                            q.push(v);
                        }
                    }
                }
            }
        };
    
        void work() {
            int n, m, q, s;
            cin >> n >> m >> q;
            Graph g(n, m);
    
            int u, v;
            for (int i = 0; i < m; i++) {
                cin >> u >> v;
                u--, v--;
                g.add_edge(u, v, i);
            }
            g.s = 0;
    
            g.dijkstra();
            g.build_DAG();
    
            while (q--) {
                int eid;
                cin >> eid;
                eid--;
                g.del_edge(eid);
    
                cout << ans << "\n";
            }
        }
    
        signed main() {
            ios::sync_with_stdio(0);
            cin.tie(0);
            int t = 1;
            // cin >> t;
            while (t--) {
                work();
            }
        }
    
        /*
        4 4 2
        1 2
        2 3
        1 4
        4 3
        2
        1
        */
        ```

???+note "[CS Academy - Chromatic Number](https://csacademy.com/contest/archive/task/chromatic-number)"
	給一張 $n$ 點 $m$ 邊的圖，邊有邊權，請選擇 $k$ 個特殊點，使 $1\to n$ 有經過這 $k$ 個特殊點的最短路徑越多越好，輸出最多能有幾條這樣的路徑以及選法有幾種
	
	$n\le 300, m\le \frac{n(n-1)}{2}$
	
	??? note "思路"
		> 建圖
	
	    - $O(n^3) \texttt{ floyd warshall}$ 
	    - $dis(u,v):$ $u$ 到 $v$ 的最短路
	    - $cnt(u,v):$ 從 $u$ 走最短路到 $v$ 有幾種走法
	
		> 狀態定義
	
	    - $f_{i,k}$ 以 $i$ 結尾選 $k$ 個節點最多能在幾個 shortest path 上
	    - $g_{i,k}$ 以 $i$ 結尾選 $k$ 個節點有幾種選法能滿足在 $f_{i,k}$ 個 shortest path 上
	
		> 轉移
	
	    - 找到 $u,v$ 滿足 $1 \rightarrow v \rightarrow u \rightarrow n$
	    - $f_{u,k}=\max \begin{cases} f_{v,k-1}\times cnt(v,u) \\ f_{u,k} \end{cases}$
	    - $g_{u,k}$
	    	- $\texttt{if }f_{v,k-1}\times cnt(v,u) \texttt{ == }f_{u,k}: g_{u,k}=g_{u,k}+g_{v,k-1}$
	    	- $\texttt{else if }f_{v,k-1}\times cnt(v,u) \texttt{ > }f_{u,k}: g_{u,k}=g_{v,k-1}$
	
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
	    const int maxn = 300 + 5;
	    const int M = 1e9 + 7;
	
	    int n, m, K;
	    int dis[maxn][maxn];
	    int cnt[maxn][maxn];
	    pii dp[maxn][maxn];
	
	    void floyd() {
	        for (int k = 1; k <= n; k++) {
	            for (int i = 1; i <= n; i++) {
	                for (int j = 1; j <= n; j++) {
	                    if (dis[i][j] == dis[i][k] + dis[k][j]) {
	                        cnt[i][j] += cnt[i][k] * cnt[k][j];
	                    } else if (dis[i][j] > dis[i][k] + dis[k][j]) {
	                        dis[i][j] = dis[i][k] + dis[k][j];
	                        cnt[i][j] = cnt[i][k] * cnt[k][j];
	                    }
	                }
	            }
	        }
	    }
	
	    void init() {
	        cin >> n >> m >> K;
	        int u, v, w;
	
	        for (int i = 1; i <= n; i++) {
	            for (int j = 1; j <= n; j++) {
	                dis[i][j] = INF;
	            }
	            dis[i][i] = 0;
	        }
	
	        for (int i = 1; i <= m; i++) {
	            cin >> u >> v >> w;
	            dis[u][v] = dis[v][u] = min(w, dis[u][v]);
	            cnt[u][v] = cnt[v][u] = 1;
	        }
	    }
	
	    void solve() {
	        floyd();
	
	        vector<pii> ord;
	        for (int i = 1; i <= n; i++) {
	            ord.pb({dis[1][i], i});
	            cnt[i][i] = 1;
	        }
	        sort(ALL(ord));  // sort by distance
	
	        // dp init
	        for (int i = 0; i < n; i++) {
	            int u = ord[i].S;
	            if (dis[1][u] + dis[u][n] != dis[1][n]) continue;
	            dp[u][1] = {cnt[1][u], 1};
	        }
	
	        // (v -> u) 一定是 v 先被走到再來才是 u，距離是一種可以判斷先後的好方法
	        for (int i = 0; i < ord.size(); i++) {
	            int u = ord[i].S;
	            if (dis[1][u] + dis[u][n] != dis[1][n]) continue;  // check u
	
	            for (int j = 0; j < i; j++) {
	                int v = ord[j].S;
	                if (dis[1][v] + dis[v][u] + dis[u][n] != dis[1][n])
	                    continue;  // check v
	
	                for (int k = 2; k <= K; k++) {
	                    pii tmp;
	                    tmp.F = dp[v][k - 1].F * cnt[v][u];
	                    tmp.S = dp[v][k - 1].S;
	
	                    if (tmp.F > dp[u][k].F)
	                        dp[u][k] = tmp;
	                    else if (tmp.F == dp[u][k].F) {
	                        dp[u][k].S += tmp.S;
	
	                        if (dp[u][k].S >= M)
	                            dp[u][k].S -= M;
	                    }
	                }
	            }
	        }
	
	        pii res = {0, 0};
	        // 以 u 結尾，還缺少到 u -> n 這段，補起來
	        // 可是 n 結尾上面有算過了阿? 不一定會以 n 結尾 但最短路可延續至 n
	        for (int i = 1; i <= n; i++) {
	            if (dis[1][i] + dis[i][n] != dis[1][n])
	                continue;
	
	            dp[i][K].F *= cnt[i][n];
	
	            if (dp[i][K].F > res.F)
	                res = dp[i][K];
	            else if (dp[i][K].F == res.F) {
	                res.S += dp[i][K].S;
	
	                if (res.S >= M)
	                    res.S -= M;
	            }
	        }
	
	        cout << res.F << " " << res.S << "\n";
	    }
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        // cin >> t;
	        while (t--) {
	            init();
	            solve();
	        }
	    }
	    ```

???+note "[CSES - Visiting Cities](https://cses.fi/problemset/task/1203)"

	給 $n$ 點 $m$ 邊正權有向圖，從 $1\to n$ 判斷每個邊是否在每個最短路徑上
	
	$n\le 10^5, m\le 2\times 10^5$
	
	??? note "思路"
		建立 shotest path DAG，進行 DAG DP:
		
		- $dp(1 \to u):$ $1\to u$ 是最短路的路徑方法數
		
		- $dp(u \to n):$ $u\to n$ 是最短路的路徑方法數
		
		判斷 $dp(1 \to u)\times dp(v\to n)==dp(s\to t)$，是話就是在最短路徑上
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	
	    using namespace std;
	
	    const int INF = 2e18;
	    const int MAXN = 3e5 + 5;
	    const int M = 2147483647;
	    int n, m;
	    vector<pii> G[MAXN];
	    vector<pii> R[MAXN];
	    vector<int> D[MAXN];
	    vector<int> P[MAXN];
	    int in[MAXN], rv[MAXN];
	
	    vector<int> dijkstra(int source, vector<pii> *G) {
	        vector<int> dis(n + 1, INF);
	        priority_queue<pii, vector<pii>, greater<pii>> pq;
	
	        pq.push({0, source});
	        while (pq.size()) {
	            auto [x, u] = pq.top();
	            pq.pop();
	
	            if (dis[u] != INF) continue;
	            dis[u] = x;
	
	            for (auto [v, w] : G[u]) {
	                pq.push({x + w, v});
	            }
	        }
	        return dis;
	    }
	
	    void build(vector<int> &dis) {
	        for (int i = 1; i <= n; i++) {
	            for (auto [v, w] : G[i]) {
	                if (dis[v] == dis[i] + w) {
	                    D[i].push_back(v);
	                    P[v].push_back(i);
	                    in[v]++;
	                    rv[i]++;
	                }
	            }
	        }
	    }
	
	    vector<int> topo(int source, int *in, vector<int> *D) {
	        queue<int> q;
	        vector<int> inn(n + 1);
	        vector<int> dp(n + 1);
	        dp[source] = 1;
	
	        for (int i = 1; i <= n; i++) {
	            if (in[i] == 0) q.push(i);
	            inn[i] = in[i];
	        }
	
	        while (q.size()) {
	            int u = q.front();
	            q.pop();
	
	            for (auto v : D[u]) {
	                dp[v] = (dp[v] + dp[u]) % M;
	                inn[v]--;
	                if (inn[v] == 0) q.push(v);
	            }
	        }
	
	        return dp;
	    }
	
	    signed main() {
	        cin >> n >> m;
	        int u, v, w;
	        for (int i = 0; i < m; i++) {
	            cin >> u >> v >> w;
	            G[u].push_back({v, w});
	            R[v].push_back({u, w});
	        }
	        vector<int> dis = dijkstra(1, G);
	        vector<int> rev = dijkstra(n, R);
	        build(dis);
	
	        vector<int> dp1 = topo(1, in, D);
	        vector<int> dp2 = topo(n, rv, P);
	
	        int tot = dp1[n];
	        vector<int> res;
	        for (int i = 1; i <= n; i++) {
	            int cur = (dp1[i] * dp2[i]) % M;
	            if (cur == tot) {
	                res.push_back(i);
	            }
	        }
	
	        cout << res.size() << "\n";
	        for (int i = 0; i < res.size(); i++) {
	            cout << res[i] << " ";
	        }
	    }
	    ```

???+note "CSES - Visiting Cities 變化"

	給 $n$ 點 $m$ 邊正權有向圖，從 $1\to n$ 判斷每個邊是哪種 $\texttt{type}$
	
	- $\texttt{type 1: }$是否在每個最短路徑上 
	
	- $\texttt{type 2: }$至少有在一個最短路徑上
	
	- $\texttt{type 3: }$根本沒有在最短路徑上
	
	$n\le 10^5, m\le 2\times 10^5$
	
	??? note "思路"
	
		建立 shotest path DAG，進行 DAG DP
		
		- $\texttt{type 1: }$ 判斷 $dp(1 \to u)\times dp(v\to n)==dp(s\to t)$
	
	    - $\texttt{type 2: }$ 在 DAG 上的邊
	
	    - $\texttt{type 3: }$ 不在 DAG 上的邊

### 建圖/分層

???+note "[LOJ #3964. 「APIO2023」赛博乐园](https://loj.ac/p/3964)"
	給一張 $n$ 點 $m$ 無向圖，邊帶權，為 $c[i]$，一開始在點 $0$，你要去點 $t$。每個點有一個能力 $arr[i]$
    
    - $arr[i] = 0$，可讓當前已走的距離設為 $0$
    
    - $arr[i] = 1$，沒任何作用
    
    - $arr[i] = 2$，可讓當前已走的距離除以 $2$ 
    
    除了點 $t$ 外，所有點都可以重複走，只要走到點就可使用 $arr[i]$，「除以 $2$ 」的能力總共只能使用 $k$ 次。只要抵達 $t$ 點就代表走到終點，問抵達 $t$ 點的最短距離
    
    $n, m\le 10^5,k\le 10^6,c[i]\le 10^9$
    
    ??? note "思路"
    	我們可以將題目的圖反著做，即從點 $t$ 開始，跑回點 $0$，這樣就可以建立分層圖
    	
    	- 原先遇到能力為 $0$ 的點，會把當前距離設置為 $0$
    		- 那麼現在遇到能力為 $0$ 的點，相當於讓之後走過的所有邊權都變成 $0$
    
    	- 原先遇到能力為 $2$ 的點，會把當前距離減半
    		- 那麼現在遇到能力為 $2$ 的點，相當於讓之後走過的所有邊權都變成原來的一半
    	
    	第 $k$ 層為當前已使用「除以 $2$ 」的能力 $k$ 次。我們發現，直接讓第 $k$ 層上 $u$ 和 $v$ 之間的邊權為原圖上 $u$ 和 $v$ 之間邊權的 $\displaystyle \frac{1}{2^k}$ ，就能滿足能力為 $2$ 的點。
    	
    	至於能力為 $0$ 的點，我們可以新建一層 $K+1$ 層，讓這一層內 $u$ 和 $v$ 之間的邊權都改成 $0$。然後讓所有 $arr[v]=0$ 的 node$(k,v)$ 直接建立單向邊到 $K+1$ 層即可。
    	
    	- Node (u, k) → Node (v, k) 1/2^k
    	
        - Node (u, k) → Node (v, k) 0 if (k=K+1)
        
        - Node (u, k) → Node (v, k+1) w * 1/2^(k+1) if arr[u]=2
        
        - Node (u, k) → Node (u, K+1) 0 if arr[u]=0 
    	
    	我們發現，事實上用一些次優惠政策之後，最短路會變的很低，遠遠低於精度。具體來說，本題中最短路最大不超過邊數乘邊權最大值，即 $10^5\times 10^9=10^{14}$，只需讓這個數除以 $70$ 次 $2$ 就可以掉到 $10^{−7}$ 以下（$8\times 10^{−8}$）
    	
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
    
        #include "cyberland.h"
        #define pii pair<int, double>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()
    
        using namespace std;
    
        const double INF = 1000000000000000.00;
    
        struct Graph {
            int n, K, cnt;
            vector<vector<pii>> G;
            vector<vector<int>> id;
            vector<double> dis;
            vector<int> vis;
    
            void init(int _n, int _k) {
                n = _n, K = _k;
                id.resize(K + 1);
                cnt = 0;
    
                // (id + 1) % n == 0 -> cyberland
                // k != 0
                // id / n -> k
                for (int i = 0; i < K + 1; i++) {
                    id[i].resize(n);
                    for (int j = 0; j < n; j++) {
                        id[i][j] = cnt++;  // id[k][u]
                    }
                }
                G.resize(cnt);
                dis = vector<double>(cnt, INF);
                vis = vector<int>(cnt);
            }
    
            void add_edge(int u, int uk, int v, int vk, double w) {
                int id1 = id[uk][u];
                int id2 = id[vk][v];
    
                G[id1].pb({id2, w});
            }
    
            void dijkstra(int s) {
                priority_queue<pair<double, int>, vector<pair<double, int>>, greater<pair<double, int>>> pq;
                pq.push({0, id[0][s]});  // cyberland 為起點
                dis[id[0][s]] = 0;
    
                while (pq.size()) {
                    auto [dis_u, u] = pq.top();
                    pq.pop();
    
                    if ((u % n) == (s % n) && (u / n) != 0) continue;
                    if (vis[u]) continue;
                    vis[u] = 1;
    
                    for (auto [v, w] : G[u]) {
                        if ((v % n) == (s % n) && (v / n) != 0) continue;
                        if (dis[v] > dis[u] + w) {
                            dis[v] = dis[u] + w;
                            pq.push({dis[v], v});
                        }
                    }
                }
            }
    
            double cal() {
                double ans = INF;
                for (int i = 0; i < K + 1; i++) {
                    int u = id[i][0];
                    ans = min(ans, dis[u]);
                }
                if (ans == INF) return -1;
    
                return ans;
            }
        };
    
        /*
        cyberland 不能去 relax 別人
        Node (u, k) -> Node (v, k) 1/2^k
        Node (u, k) -> Node (v, k) 0 if (k=K+1)
        Node (u, k) -> Node (v, k+1) w * 1/2^(k+1) if arr[u]=2
        Node (u, k) -> Node (u, K+1) 0 if arr[u]=0
        */
    
        double solve(int N, int M, int K, int H, vector<int> x, vector<int> y, vector<int> c, vector<int> arr) {
            K = min(K, 70);
            vector<vector<pii>> G(N);
            for (int i = 0; i < M; i++) {
                int u = x[i], v = y[i], w = c[i];
                G[u].pb({v, w});
                G[v].pb({u, w});
            }
            K++;
    
            Graph g;
            g.init(N, K);
    
            double cnt = 1;
            for (int k = 0; k < K + 1; k++) {
                if (k == K) {
                    for (int i = 0; i < N; i++) {
                        for (auto [v, w] : G[i]) {
                            g.add_edge(i, k, v, k, 0);
                        }
                    }
                    continue;
                }
                for (int i = 0; i < N; i++) {
                    for (auto [v, w] : G[i]) {
                        g.add_edge(i, k, v, k, (double)w * cnt);
                    }
                }
                cnt *= 0.5;
            }
            cnt = 1;
            for (int k = 0; k < K - 1; k++) {
                cnt *= 0.5;
                for (int i = 0; i < N; i++) {
                    for (auto [v, w] : G[i]) {
                        if (arr[i] == 2) {
                            g.add_edge(i, k, v, k + 1, (double)w * cnt);
                            }
                    }
                }
            }
            for (int k = 0; k < K; k++) {
                for (int i = 0; i < N; i++) {
                    if (arr[i] == 0) {
                        g.add_edge(i, k, i, K, 0);
                    }
                }
            }
    
            g.dijkstra(H);
    
            return g.cal();
        }
        ```
    ??? note "full code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define pii pair<int, long double>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()
    
        using namespace std;
    
        const long double INF = 1000000000000000.00;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;
    
        struct Graph {
            int n, K, cnt;
            vector<vector<pii>> G;
            vector<vector<int>> id;
            vector<double> dis;
            vector<int> vis;
    
            void init(int _n, int _k) {
                n = _n, K = _k;
                id.resize(K + 1);
                cnt = 0;
    
                // (id + 1) % n == 0 -> cyberland
                // k != 0
                // id / n -> k
                for (int i = 0; i < K + 1; i++) {
                    id[i].resize(n);
                    for (int j = 0; j < n; j++) {
                        id[i][j] = cnt++;  // id[k][u]
                    }
                }
                G.resize(cnt);
                dis = vector<double>(cnt, INF);
                vis = vector<int>(cnt);
            }
    
            void add_edge(int u, int uk, int v, int vk, double w) {
                int id1 = id[uk][u];
                int id2 = id[vk][v];
    
                G[id1].pb({id2, w});
            }
    
            void dijkstra(int s) {
                priority_queue<pair<double, int>, vector<pair<double, int>>, greater<pair<double, int>>> pq;
                pq.push({0, id[0][s]});  // cyberland 為起點
                dis[id[0][s]] = 0;
    
                while (pq.size()) {
                    auto [dis_u, u] = pq.top();
                    pq.pop();
    
                    if ((u % n) == (s % n) && (u / n) != 0) continue;
                    if (vis[u]) continue;
                    vis[u] = 1;
    
                    for (auto [v, w] : G[u]) {
                        if ((v % n) == (s % n) && (v / n) != 0) continue;
                        if (dis[v] > dis[u] + w) {
                            dis[v] = dis[u] + w;
                            pq.push({dis[v], v});
                        }
                    }
                }
            }
    
            double cal() {
                double ans = INF;
                for (int i = 0; i < K + 1; i++) {
                    int u = id[i][0];
                    ans = min(ans, dis[u]);
                }
                if (ans == INF) return -1;
    
                return ans;
            }
        };
    
        /*
        cyberland 不能去 relax 別人
        Node (u, k) -> Node (v, k) 1/2^k
        Node (u, k) -> Node (v, k) 0 if (k=K+1)
        Node (u, k) -> Node (v, k+1) w * 1/2^(k+1) if arr[u]=2
        Node (u, k) -> Node (u, K+1) 0 if arr[u]=0
        */
    
        double solve(int N, int M, int K, int H, vector<int> x, vector<int> y, vector<int> c, vector<int> arr) {
            K = min(K, 70);
            vector<vector<pii>> G(N);
            for (int i = 0; i < M; i++) {
                int u = x[i], v = y[i], w = c[i];
                G[u].pb({v, w});
                G[v].pb({u, w});
            }
            K++;
    
            Graph g;
            g.init(N, K);
    
            double cnt = 1;
            for (int k = 0; k < K + 1; k++) {
                if (k == K) {
                    for (int i = 0; i < N; i++) {
                        for (auto [v, w] : G[i]) {
                            g.add_edge(i, k, v, k, 0);
                        }
                    }
                    continue;
                }
                for (int i = 0; i < N; i++) {
                    for (auto [v, w] : G[i]) {
                        g.add_edge(i, k, v, k, (double)w * cnt);
                    }
                }
                cnt *= 0.5;
            }
            cnt = 1;
            for (int k = 0; k < K - 1; k++) {
                cnt *= 0.5;
                for (int i = 0; i < N; i++) {
                    for (auto [v, w] : G[i]) {
                        if (arr[i] == 2) g.add_edge(i, k, v, k + 1, (double)w * cnt);
                    }
                }
            }
            for (int k = 0; k < K; k++) {
                for (int i = 0; i < N; i++) {
                    if (arr[i] == 0) g.add_edge(i, k, i, K, 0);
                }
            }
    
            g.dijkstra(H);
    
            return g.cal();
        }
    
        void init() {
            int n, m, k, h;
            cin >> n >> m >> k >> h;
            vector<int> arr(n);
            vector<int> x(n);
            vector<int> y(n);
            vector<int> c(n);
            for (int i = 0; i < n; i++) cin >> arr[i];
            for (int i = 0; i < m; i++) cin >> x[i] >> y[i] >> c[i];
    
            cout << fixed << setprecision(12) << solve(n, m, k, h, x, y, c, arr) << "\n";
        }
    
        signed main() {
            ios::sync_with_stdio(0);
            cin.tie(0);
            int t = 1;
            cin >> t;
            while (t--) {
                init();
            }
        }
        ```

???+note "CSES - flight discount 變化"
	輸入一個 $n$ 點 $m$ 邊的有向圖，每條邊都有權重 $w(u,v)$。若連續走兩條邊 $a\to b\to c$，本來需花 $w(a,b)+w(b,c)$，使用優惠券可以將花費改成 $w(b,c)\times 2$，優惠券只能用 $k$ 次。問 $1\to n$ 的最小花費
	
	??? note "思路"
		把圖複製 $k\times 2$ 層，三層三層一組，第 $k$ 層若原圖 $G$ 有邊 $(a,b,w_{a,b}),(b,c,w_{b,c})$ 就連接 $(a_k,b_{k+1},0),(b_{k+1},c_{k+2},2\times w_{b,c})$
	
		<figure markdown>
	      ![Image title](./images/31.png){ width="300" }
	    </figure>


???+note "[2021 附中模競 II 惡地之路](https://drive.google.com/file/d/1ISO-o4DrQmbuqVVAgxeVQEO3ifMvcy01/view)"
	給一張 $n$ 點 $m$ 邊無向圖，令 $s$ 到節點 $i$ 走 $k$ 步的最短距離是 $d(i,k)$，對於每個 $i$ 求 $\min \{ d(i,k) \times k \}$
	
	$n\le 2000,m\le 3\times 10^4$
	
	??? note "思路"
	
		此方法並非滿分解，滿分解在<a href="/wiki/graph/Tree/#_1" target="_blank">這裡</a>
		
		---
		
		把每個節點都複製 $n$ 份
		
		如果本來有一條邊是 $(u,v)$，那就對所有 $1\le i < n$ 蓋
		
		- $u$ 的第 $i$ 個點到 $v$ 的第 $i+1$ 個點（有向）
		- $v$ 的第 $i$ 到 $u$ 的第 $i+1$ 個點（有向）
	
		這樣走到某個節點的第 $i$ 個點的路徑長度一定是 $i-1$
		
		$n^2$ 個點，$2nm$ 個邊做最短路徑，$O(nm\log⁡ nm)$
		
		枚舉 $k=1\ldots n$，對於第 $k$ 層拉出來求最小的 $dis_k$ 
		
		$$ans = \min\limits_{k=1\ldots n} \{dis_k\times k\}$$

???+note "[2023 全國賽模擬賽 pI. 交通優惠券 (voucher)](https://codeforces.com/gym/104830/attachments/download/23302/zh_TW.pdf#page=26)"
    給一張 n 點 m 邊的無向圖，邊有正權，有兩個特殊點 a, b，在經過特殊點後可將之後經過的一條邊免費，問 s 到 t 的最短路徑長度
    
    $n \le 3 \times 10^5, m \le 5 \times 10^5$
    
    ??? note "思路"
    	考慮只有一個特殊點時，有三種情況:
    	
        1. 還沒走到特殊點
        2. 走到特殊點，還沒有使用優惠卷
        3. 使用完優惠卷
    
    	我們可以建立三張圖，分別代表以上三種 case。將第一張圖的特殊點連一條邊到第二張圖的特殊點，代表已經可以使用優惠券了，再將第二張圖的每一個點連到第三張圖周圍的點，邊權為 0，走過去代表使用完了優惠券。
    	
    	對於兩個特殊點的情況，我們定義好狀態 (x, y) 為經過 x 個特殊點，使用掉 y 個優惠卷。我們就用 (0, 0), (1, 0), (1, 1), (2, 0), (2, 1), (2, 2) 這幾個狀態來建圖即可



???+note "[CSES - flight discount](https://cses.fi/problemset/task/1195)"
	給一張 $n$ 點 $m$ 邊的無向圖，邊有權重，可將其中 $k$ 條邊以半價計算，求 $1\to n$ 的最短路
	
	$n\le 10^5,m\le 2\times 10^5$
	
	??? note "思路"
		原本 graph 有 $n$ 的點，變成一個圖有 $kn$ 個點的新 graph
		
		$\texttt{node}(k, u)$ 到 $\texttt{node}(k, v)$ 的長度就是 $w(u, v)$
		
		$\texttt{node}(k-1, u)$ 到 $\texttt{node}(k, v)$ 的長度就是 $w(u, v)/2$
		
		直接跑 Dijkstra，起點 $\texttt{node}(0, 1)$ 終點 $\texttt{node}(2, n)$

???+note "[2023 TOI 一模 pD.安逸旅行路線 (jaunt)](https://drive.google.com/file/d/1_sx9DvDSjpn0RCR280MKsS_FNfrr-iqy/view)"
	有一張 $n$ 點 $m$ 邊有向圖，邊 $u \rightarrow v$ 的難度係數為 $d(u, v)$，代表如果 $u \rightarrow v$ 是路徑上的第 $k$ 條邊（1-based），則這條邊的辛苦程度是 $d(u, v)^k\mod P$，一條路徑的辛苦程度被定義為路徑上所有邊的最大辛苦程度。

	輸出 $s$ 到 $t$ 的所有路徑中，最小辛苦程度的值，若不存在請輸出 $-1$。
	
	$n\le 1000, m\le 5000, P\le 10^5$ 且 $P$ 是質數
	
	??? note "範測"
	
		=== "sample1"
		
			=== "input"
				
				```
	            3 3 11 1 3
	            1 2 3
	            2 3 2
	            1 3 9
	            ```
	        
	        === "output"
	        	
	        	```
	        	4
	        	```
	    
	    === "sample2"
	    	
	    	=== "input"
				
				```
	            3 3 11 1 3
	            1 2 2
	            2 1 1
	            1 3 7
	            ```
	        
	        === "output"
	        	
	        	```
	        	2
	        	```
	        	
		=== "sample3"
	    	
	    	=== "input"
				
				```
	            3 3 11 1 3
	            1 2 5
	            2 1 1
	            3 1 4
	            ```
	        
	        === "output"
	        	
	        	```
	        	-1
	        	```
	        	
		=== "sample4"
	    	
	    	=== "input"
				
				```
	            2 6 94949 1 2
	            1 1 2
	            1 2 12345
	            1 2 23451
	            1 2 34512
	            1 2 45123
	            1 2 51234
	            ```
	        
	        === "output"
	        	
	        	```
	        	1391
	        	```
	
	??? note "思路"
		根據費馬小定理，若 $p$ 是質數，且 $1\leq d<p$，則 $d^{p-1}\bmod p$ 一定是 $1$。
		
		也就是說，這些邊的權重每 $p-1$ 步會循環一次。
	
	    我們可以建立一張新的圖，總共有 $(p-1) \times n$ 個節點。
	
	    node$(k, i)$ 的意義表示走完的步數 $\bmod (p-1) = k$，且停在原圖的節點 $i$。
	    新的 graph 會有 $(p-1) \times m$ 條邊。
	
	    若原圖有一個邊 $(u, v)$，則在新圖中，對所有的 $k$ 加上 node$(k, u) \rightarrow$ node$((k+1)\bmod (p-1), v)$ 的邊，權重是 $d(u, v)^{(k+1)\bmod (p-1)}\bmod p$。
	
	    題目的目標是要讓最大邊權最小化，所以一種方法是二分答案 $X$，看看只走 $\leq X$ 的邊是否從起點到終點 node$(k, t)$。
	
	    另一種方法是比較類似 MST 的 Prim 演算法。
	
	    先把設定 $X=1$，看看有沒有能走到 node$(k=0\sim (p-2), t)$ 任意一個節點，
	
	    如果不行就放寬 $X=2$，再看看能多走哪些。
	
	    如果不行就放寬 $X=3$，再看看能多走哪些。
	
	    一直放寬到可以走到 node$(k=0\sim (p-2), t)$ 任一個節點。
	
	    這題的邊權重會介於 $[0, p-1]$，所以 priority_queue 可以用開 $p-1$ 個 vector 的方式來實作，讓 push / pop 時間只要 $O(1)$。
	
	    整個圖的邊數量有 $(p-1) \times m$，總時間複雜度也是 $(p-1) \times m$。
	
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
	    const int maxn = 1e3 + 5;
	    const int M = 1e9 + 7;
	
	    struct node {
	        int u, k, dis;
	    };
	
	    struct DS {
	        vector<vector<node>> pq;
	        int max_val = 0, threshold = 0;
	
	        void init(int _max_val) {
	            max_val = _max_val;
	            pq = vector<vector<node>> (max_val + 1);
	        }
	
	        void push(node x) {
	            pq[max (threshold, x.dis)].pb(x);
	        }
	
	        node get_value() {
	            while (threshold <= max_val && pq[threshold].size() == 0) threshold++;
	
	            if (threshold <= max_val && pq[threshold].size() > 0) {
	                node ret = pq[threshold].back();
	                pq[threshold].pop_back();
	                return ret;
	            }
	            else return {-1, -1, -1};
	        }
	    } pq;
	
	    int n, m, P, s, t;
	    vector<pii> G[maxn];
	    int vis[maxn][maxn];
	
	    int fpow(int a, int b, int p) {
	        int ret = 1;
	        while (b != 0) {
	            if (b & 1) ret = (ret * a) % p;
	            a = (a * a) % p;
	            b >>= 1;
	        }
	        return ret;
	    }
	
	    int Prim() {
	        pq.init(P - 1);
	        // k = [0, p - 2] k = 0 為開始那層
	        pq.push({s, 0, 0});
	        int fg = 0;
	        while (true) {
	            auto [u, k, dis] = pq.get_value();
	            if (u == -1) break;
	            if (vis[u][k] == 1) continue;
	            vis[u][k] = 1;
	            if (u == t) {
	                fg = 1;
	                break;
	            }
	            for (auto [v, w] : G[u]) {
	                int vk = (k + 1) % (P - 1);
	                int wk = fpow(w, (k + 1) % (P - 1), P);
	
	                pq.push({v, vk, wk});
	            }
	        }
	        if (fg == 1) return pq.threshold;
	        return -1;
	    }
	
	    void init() {
	        cin >> n >> m >> P >> s >> t;
	        int u, v, w;
	        for (int i = 0; i < m; i++) {
	            cin >> u >> v >> w;
	            G[u].pb({v, w});
	        }
	    }
	
	    void work() {
	        cout << Prim() << "\n";
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            init();
	            work();
	        }
	    } 
	    ```

???+note "[CF 1422 D. Returning Home](https://codeforces.com/problemset/problem/1422/D)"
	在 $n\times n$ 的 grid 上，給起點終點，還有 $m$ 個特殊點，每秒可以上下左右走一格，只要與特殊點同一個 row 或 col，可以不花時間直接傳送到特殊點，問到達終點的最少時間
	
	$n\le 10^9,m\le 10^5$
	
	??? note "提示"
		grid 的性質 : 兩點間的距離為曼哈頓距離
		
	??? note "思路"
		把特殊點所在的行和列當作點
		
		1. 特殊點向它們所在的行和列連雙向邊，花費都為 0
	    2. 起點向它所在的行列連邊，花費都為 0
	    3. 終點跟所有特殊點連邊，邊權為曼哈頓距離
	    3. 出現的行之間連雙向邊，花費為兩行之間的距離
	    4. 出現的列之間連雙向邊，花費為兩列之間的距離
	    5. 起點與終點連邊，邊權為曼哈頓距離
		
		最後的答案記得跟起點直接到終點的答案取 min
		
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
	
	    struct Graph {
	        vector<vector<pii>> G;
	        int n = 0;
	
	        int add_node() {
	            n++;
	            G.pb({});
	            return n - 1;
	        }
	
	        void add_edge(int u, int v, int w) {
	            G[u].pb({v, w});
	        }
	
	        int dijkstra(int s, int t) {
	            vector<int> dis(n, INF);
	            priority_queue<pii, vector<pii>, greater<pii>> pq;
	            pq.push({0, s});
	            dis[s] = 0;
	
	            while (pq.size()) {
	                auto [dis_u, u] = pq.top();
	                pq.pop();
	                if (dis[u] < dis_u) continue;
	                dis[u] = dis_u;
	
	                for (auto [v, w] : G[u]) {
	                    if (dis[v] > dis[u] + w) {
	                        dis[v] = dis[u] + w;
	                        pq.push({dis[v], v});
	                    }
	                }
	            }
	            return dis[t];
	        }
	    };
	
	    /*
	    1. 特殊點向它們所在的行和列連雙向邊，花費都為 0
	    2. 起點與終點向它所在的行列連邊
	    3. 出現的行之間連雙向邊，花費為兩行之間的距離
	    4. 出現的列之間連雙向邊，花費為兩列之間的距離
	    5. 起點與終點連邊，邊權為 |x_s - x_t| + |y_s - y_t|
	    */
	    int n, m;
	    pii s, t;
	    vector<pii> special;
	    vector<int> X, Y;
	    int id_X[maxn], id_Y[maxn], id_Special[maxn];
	    int id_start, id_end;
	
	    void init() {
	        cin >> n >> m;
	        cin >> s.F >> s.S >> t.F >> t.S;
	
	        for (int i = 0; i < m; i++) {
	            int x, y;
	            cin >> x >> y;
	            special.pb({x, y});
	            X.pb(x);
	            Y.pb(y);
	        }
	        X.pb(s.F);
	        X.pb(t.F);
	        Y.pb(s.S);
	        Y.pb(t.S);
	    }
	
	    void work() {
	        Graph g;
	        sort(ALL(X));
	        X.resize(unique(ALL(X)) - X.begin());
	        sort(ALL(Y));
	        Y.resize(unique(ALL(Y)) - Y.begin());
	
	        for (int i = 0; i < m; i++) {
	            id_Special[i] = g.add_node();
	        }
	
	        map<int, int> mpx, mpy;
	
	        for (int i = 0; i < X.size(); i++) {
	            id_X[i] = g.add_node();
	            mpx[X[i]] = id_X[i];
	        }
	
	        for (int i = 0; i < Y.size(); i++) {
	            id_Y[i] = g.add_node();
	            mpy[Y[i]] = id_Y[i];
	        }
	        id_start = g.add_node();
	        id_end = g.add_node();
	
	        for (int i = 0; i < m; i++) {
	            g.add_edge(id_Special[i], mpx[special[i].F], 0);
	            g.add_edge(mpx[special[i].F], id_Special[i], 0);
	        }
	
	        for (int i = 0; i < m; i++) {
	            g.add_edge(id_Special[i], mpy[special[i].S], 0);
	            g.add_edge(mpy[special[i].S], id_Special[i], 0);
	        }
	
	        for (int i = 0; i < X.size(); i++) {
	            if (i > 0) {
	                g.add_edge(id_X[i], id_X[i - 1], X[i] - X[i - 1]);
	                g.add_edge(id_X[i - 1], id_X[i], X[i] - X[i - 1]);
	            }
	        }
	
	        for (int i = 0; i < Y.size(); i++) {
	            if (i > 0) {
	                g.add_edge(id_Y[i], id_Y[i - 1], Y[i] - Y[i - 1]);
	                g.add_edge(id_Y[i - 1], id_Y[i], Y[i] - Y[i - 1]);
	            }
	        }
	
	        g.add_edge(id_start, mpx[s.F], 0);
	        g.add_edge(id_start, mpy[s.S], 0);
	
	        for (int i = 0; i < m; i++) {
	            int cost = abs(special[i].F - t.F) + abs(special[i].S - t.S);
	            g.add_edge(id_Special[i], id_end, cost);
	        }
	
	        g.add_edge(id_start, id_end, abs(s.F - t.F) + abs(s.S - t.S));
	
	        cout << g.dijkstra(id_start, id_end) << "\n";
	    }
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        // cin >> t;
	        while (t--) {
	            init();
	            work();
	        }
	    }
		```

???+note "[CF 1846 G. Rudolf and CodeVid-23](https://codeforces.com/contest/1846/problem/G)"
	以下提到的 $01$ bit-string 長度皆為 $n$。給一個 $01$ bit-string，代表目前有的症狀，有 $m$ 個藥可以使用，每個藥有緩解的 $01$ bit-string $a_i$，與副作用 $01$ bit-string $b_i$，與花費 $d_i$。每種藥吃完即消失。問最少花費使所有症狀消失
	
	$n\le 10,m\le 1000$
	
	??? note "思路"
		這題的關鍵是能不能想出可以用圖論的觀點看。
		
		將每種 $01$ bit-string 的狀態看成一個點，依照題意將狀態之間連有向邊，邊權為 $d_i$，答案就是從一開始的 $01$ bit-string 到 $0$ 的最短路
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<long long, long long>
	    #define pb push_back
	    #define mk make_pair
	    #define F first
	    #define S second
	    #define ALL(x) x.begin(), x.end()
	
	    using namespace std;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    struct data {
	        int a, b, d;
	    } a[maxn];
	
	    struct Graph {
	        vector<vector<pii>> G;
	        int n = 0;
	
	        int add_node() {
	            n++;
	            G.pb ({});
	            return n - 1;
	        }
	
	        void add_edge(int u, int v, int w) {
	            G[u].pb({v, w});
	        }
	
	        int dijkstra(int s, int t) {
	            vector<int> dis (n, INF);
	            priority_queue<pii, vector<pii>, greater<pii>> pq;
	            pq.push ({0, s});
	            dis[s] = 0;
	
	            while (pq.size ()) {
	                auto [dis_u, u] = pq.top(); pq.pop();
	                if (dis[u] < dis_u) continue;
	                dis[u] = dis_u;
	
	                for (auto [v, w] : G[u]) {
	                    if (dis[v] > dis[u] + w) {
	                        dis[v] = dis[u] + w;
	                        pq.push ({dis[v], v});
	                    }
	                } 
	            }
	            if (dis[t] == INF) return -1;
	            return dis[t];
	        }
	        void clear() {
	            for (int i = 0; i < n; i++) {
	                G[i].clear();
	            }
	        }
	    } g;
	
	    int n, m;
	    int now;
	    int id[(1 << 20)];
	
	    void init() {
	        cin >> n >> m;
	        string s;
	        cin >> s;
	        now = 0;
	        for (int i = n - 1; i >= 0; i--) {
	            now += (s[i] - '0') * (1 << i);
	        }
	        for (int t = 0; t < m; t++) {
	            cin >> a[t].d;
	            string s1, s2;
	            cin >> s1 >> s2;
	            a[t].a = 0, a[t].b = 0;
	            for (int i = n - 1; i >= 0; i--) {
	                a[t].a += (s1[i] - '0') * (1 << i);
	            }
	            for (int i = n - 1; i >= 0; i--) {
	                a[t].b += (s2[i] - '0') * (1 << i);
	            }
	        }
	    }
	
	    void solve() {
	        for (int mask = 0; mask < (1 << n); mask++) {
	            for (int i = 0; i < m; i++) {
	                int S = mask & (((1 << 20) - 1) ^ a[i].a);
	                S |= a[i].b;
	                g.add_edge(id[mask], id[S], a[i].d);
	            }
	        } 
	        int ans = g.dijkstra(id[now], id[0]);
	        cout << ans << '\n';
	    }
	
	    signed main() {
	        for (int mask = 0; mask < (1 << 20); mask++) {
	            id[mask] = g.add_node();
	        } 
	        int t = 1;
	        cin >> t;
	        while (t--) {
	            g.clear();
	            init();
	            solve();
	        }
	    } 
	    ```

### 建立虛點

???+note "[JOI 2021 Final - Robot](https://loj.ac/p/3471)"
    給一張 $n$ 點 $m$ 邊無向圖，邊有顏色 $c_i$ 與權值 $w_i$，每條邊可花 $P_i$ 變顏色（只限變一次）。想從 $u$ 走到 $v$ 若且唯若 $u$ 的鄰邊只有 $u\rightarrow v$ 有該種顏色。問從 $1$ 走到 $n$ 的最小花費。
    
    $n\le 10^5,m\le 2\times 10^5, 1\le c_i\le m, 1\le w_i\le 10^9$
    
    ??? note "範例"
    	<figure markdown>
          ![Image title](./images/145.png){ width="400" }
        </figure>
    	
    ??? note "思路"
    	令一個點 u 他周圍顏色為 c 的 w 總和為 $S_{u,c}$。我們首先可以想到，對於 u 走到 v，若 (u, v) 的顏色 distinct，那我們的花費就是 0；否則，我們有兩種方法:
    	
    	1. 改變 (u, v) 本身的顏色，花費為 $w$
    
    	2. 改變跟 (u, v) 同色的邊，花費為 $S_{u,c}-w$
    	
    	這樣就結束了嗎，不盡如此。我們發現當走了 A 到 B 到 C，滿足 (A, B) 與 (B, C) 的顏色皆相同，且 (A, B) 是走 case 1，而 (B, C) 是走 case 2，那我們中間的邊會重複算。
    	
    	<figure markdown>
          ![Image title](./images/143.png){ width="400" }
        </figure>
        
        以上圖來說，第一次以 A 為中心，對 (A, B) 用 case1 算了 w(A, B)；而第二次以 B 為中心，對 (B, C) 用 case2 算了 w(B, H) + w(B, G) + w(A, B)。發現到 w(A, B) 被重複算兩次。 
        
        解決的方法，我們可以建立幫每個點的每個顏色都建立虛點，假設今天是 $B$ 點的虛點 $B_{c}$，就直接將 $A \to B_c$ 的權重設為 0，而 $B_c \to C$ 的權重設為 $S_{B, c} - w(B, C)$。
        
        <figure markdown>
          ![Image title](./images/144.png){ width="400" }
        </figure>
        
        ---
        
        Q1: 對於 case1，為何一定找的到一種顏色來換?
        
        A1: 因為邊最多只有 m 條，而顏色有 m 種，不管怎麼樣至少一定有一種顏色沒被挑到。
        
        Q2: 會不會在進行 case2 時，將某條邊 e 換掉了我們等等要用的顏色後，等等需要用到 e 時，所收到的顏色是錯誤的?
        
        A2: 不會，如果待會還要用到，那不如在目前這一步直接用 case1 走 e 就好。
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define ALL(v) begin(v), end(v)
        #define All(v, l, r) &v[l], &v[(r) + 1]
        using i64 = int64_t;
        using db = double;
        using std::cin;
        using std::cout;
        constexpr int N = 1e5 + 5, M = 5e5 + 5;
        constexpr i64 inf = 1e18;
    
        int n, m, vt;
        std::array<std::map<int, std::vector<std::pair<int, i64> > >, N> vec;
        std::array<std::map<int, i64>, N> sum;
        std::array<std::vector<std::pair<int, i64> >, M> G;
    
        namespace Dij {
        std::array<i64, M> dis;
        std::array<bool, M> vis;
        std::priority_queue<std::pair<i64, int>, std::vector<std::pair<i64, int> >, std::greater<std::pair<i64, int> > > q;
    
        auto dij(int s) {
            std::fill(All(dis, 1, vt), inf);
            q.emplace(0, s), dis[s] = 0;
    
            while (!q.empty()) {
                auto u = q.top().second;
                q.pop();
                if (vis[u]) continue;
                vis[u] = 1;
                for (auto [v, w] : G[u]) {
                    if (dis[v] > dis[u] + w) {
                        dis[v] = dis[u] + w;
                        if (!vis[v]) q.emplace(dis[v], v);
                    }
                }
            }
            return (dis[n] == inf ? -1 : dis[n]);
        }
        }
    
        auto main() -> int {
            std::ios::sync_with_stdio(false);
            cin.tie(nullptr), cout.tie(nullptr);
    
            cin >> n >> m, vt = n;
            for (auto i = 1, u = 0, v = 0, w = 0, c = 0; i <= m; ++i) {
                cin >> u >> v >> c >> w;
                vec[u][c].emplace_back(v, w), vec[v][c].emplace_back(u, w);
                sum[u][c] += w, sum[v][c] += w;
            }
            for (auto u = 1; u <= n; ++u) {
                for (auto [c, cur] : vec[u]) {
                    auto s = sum[u][c];
                    vt++;
                    for (auto [v, w] : cur) {
                        if (cur.size() == 1)
                            G[u].emplace_back(v, 0);
                        else
                            G[u].emplace_back(v, std::min(w, s - w));
                        G[v].emplace_back(vt, 0), G[vt].emplace_back(v, s - w);
                    }
                }
            }
    
            cout << Dij::dij(1) << "\n";
            return 0;
        }
        ```

???+note "[USACO Gold 2021 January - Telephone](https://www.luogu.com.cn/problem/P7297)" 
	給 $n$ 跟一個 $k\times k$ 的 matrix $S$，每個點有一個權值 $b_i$。$i$ 能走到 $j$ 當且僅當 $S_{b[i],b[j]}=1$，花費 $\text{cost}=|i-j|$。求從 $1\rightarrow n$ 最少要多少 $\text{cost}$
	
	$n\le 5\times 10^4,k\le 50,1\le b_i\le k$
	
	??? note "思路"
	    【觀察】:
	    
	    顯然 $O(n^2)$ 的複雜度我們無法接受，考慮到 k 只有 50，我們從這裡下手。觀察到 $\text{cost}=|i-j|\rightarrow$ 可以想成每次都移動 $1$ 格，也就是 從 $i\rightarrow i+1 \texttt{ or } i-1$。
	    
	    一個點 i 要走到 j，我們只需要去看他的 b[i] 與 b[j] 即可，優點是他們的範圍在 [1, k]。
	    
	    【作法: 分層圖】:
	    
	    依照範圍，我們猜到大概是要建一個 $O(nk)$ 的分層圖。我們想一下 node(u, b) 會代表的是什麼，u 一定是當前的點，而 b 我們依照上面的觀察可以得知就是上一個點的 b[i]。具體來說，u 代表目前的位置（不是真正在 u，而是目前的 cost 增加或減到 u），b 代表從上個真正點的 $b_i$。轉移如下:
	
	    node(u, b) = $\begin{cases} \texttt{node(u + 1, b)}+1 \\ \texttt{node(u - 1, b)}+1 \\ \texttt{node(u, b[u])}+0 \end{cases}$
	
	    其中 node (u, b) -> node (u, b[u]) 代表的是從虛點走到真正 u 的點。我們就可以做 dijkstra 了。

???+note "[LOJ #2335. 「JOI 2017 Final」足球](https://loj.ac/p/2335)"
    有 $n$ 個球員站在 Grid 上求球從 $a_1$ 踢到 $a_n$ 的最小 $cost$
    
    - 球員踢球(上下左右) $A\times p + B$
    
    - 球員移動(上下左右) $cost=C$
    
    - 放下球 $cost=0$
    
    - 拿起球 $cost = 0$
    
    ??? note "思路"
    
        > - $0,1,2,3$ 上下左右 (自飛)
        
        > - $4$ 停止 (自飛)
        
        > - $5$ 帶飛 (往 random direction)
    
        - 自飛的球
        	- 繼續移動 $0,1,2,3\rightarrow 0,1,2,3 :A$ 往自己的方向 
        	- 被球員撿到 $4\rightarrow 5: dis_{i,j}\times C$ 
        		- $dis_{i,j}$ 為最近的球員到 $(i,j)$ 的距離
        	- 停下來  $0,1,2,3\rightarrow 4:0$  
    
        - 帶飛的球
        	- 繼續跟著球員走 $5 \rightarrow 5:C$  四個方向
        	- 被球員踢出去 $5 \rightarrow 0,1,2,3:B$ 
        	- ~~停下來~~ 停下來，撿起來，浪費時間

???+note "[CF 1860 E. Fast Travel Text Editor](https://codeforces.com/contest/1860/problem/E)"
	
	給一個字串 S，每個相鄰兩個字母叫一個位置，有 q 筆詢問從位置 s → t 的最少操作次數是多少。一次操作可執行以下三個選項之一：
	
	- 往左移一格
	
	- 往右移一格
	
	- 傳送到同樣的相鄰兩個字母同樣的位置
	
	$2\le |S|\le 5\times 10^4,1\le q\le 5\times 10^4$
	
	??? note "思路"
		觀察 : 相鄰字串的組合只有 26 * 26 種，可以建圖做 dijkstra
		
		考慮建邊，對於一個位置，他可以 :
		
		- 傳送到相同的字串，cost = 1
	
		- 往左走或往右走，cost = 1
	
		因為在相同的字串之間建完全圖太費時了，我們考慮對於每種字串組合都建一個虛點，分別與同種字串組合的 node 連邊。考慮邊權，當從一個 index $i$ 走到虛點再走到 index $j$ 會花 cost = 1 很難實作，我們不如將與虛點連接的邊權都設為 1，最後答案再除以 2 就好了，這麼做的話往左走或往右走的邊權也要設為 2。
		
		<figure markdown>
	      ![Image title](./images/54.png){ width="400" }
	    </figure>
	    
	    考慮 query 的部分，每次重跑一次 dijkstra 會太久，我們可以分成兩種 case，有至少做一次傳送與完全沒做傳送。
	    
	    有做一次傳送我們就可以枚舉有做傳送的虛點，答案就是 dis(s → 虛點) + dis(虛點 → t) 再除以 2。（因為一定只能透過相同字串組合的點進去虛點，所以可以保整有經過兩次）。到虛點的部分可以預處理所有虛點當起點的 dijkstra。
	    
	    完全沒做的部分就直接算 index s 跟 index t 的距離就好了
	    
	    > 類似題 : <https://codeforces.com/contest/1301/problem/F>
	    
	??? note "code(from abc)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	    #define ll long long
	    #define pb push_back
	    #define all(x) (x).begin(), (x).end()
	    #define pii pair<int, int>
	    const int mod = 998244353, N = 6e4, M = 26 * 26;
	
	    int dis[M][N];
	    vector <pii> adj[N];
	
	    void build(int s, int id) {
	        fill(dis[id], dis[id] + N, N);
	        queue <int> q;
	        dis[id][s] = 0;
	        q.push(s);
	        while (!q.empty()) {
	            int v = q.front(); q.pop();
	            for (auto [u, w] : adj[v]) if (dis[id][u] > dis[id][v] + w) {
	                dis[id][u] = dis[id][v] + w;
	                q.push(u);
	            }
	        }
	    }
	
	    void solve() {
	        string s;
	        cin >> s;
	        int n = s.length();
	        for (int i = 0; i < n - 1; ++i) {
	            int x = (s[i] - 'a') * 26 + (s[i + 1] - 'a') + n; // 虛點
	            adj[i].emplace_back(x, 1), adj[x].emplace_back(i, 1);
	        }
	        for (int i = 0; i + 2 < n; ++i) { // 相鄰的建邊
	            adj[i].emplace_back(i + 1, 2), adj[i + 1].emplace_back(i, 2);
	        }
	        for (int i = 0; i < M; ++i) { // 預處理
	            build(i + n, i);
	        }
	        int q; cin >> q;
	        while (q--) {
	            int s, t; cin >> s >> t, --s, --t;
	            int ans = abs(s - t);
	            for (int i = 0; i < M; ++i) {
	                ans = min(ans, (dis[i][s] + dis[i][t]) / 2);
	            }
	            cout << ans << '\n';
	        }
	    }
	
	    int main() {
	        ios::sync_with_stdio(false), cin.tie(0);
	        int t = 1;
	        // cin >> t;
	        while (t--) {
	            solve();
	        }
	    }
	    ```

???+note "[JOI 2024 Final 建设工程 2](https://loj.ac/p/4090)"
    給一張 $n$ 點 $m$ 邊無向圖，第 $i$ 條邊為 $(u_i,v_i)$，邊權 $w_i$。問有幾種 pair$(u,v)$ 滿足 $u < v$ 且在 $u,v$ 之間建一條邊權 $\ell$ 的雙向邊，可以使起點 $s$ 到終點 $t$ 的最短路距離 $\le k$

    $n,m\le 2\times 10^5, 1\le \ell,w_i \le 10^9,1\le k\le 10^{15}$
    
    ??? note "思路"
    	首先，若 $\text{dis}(s \rightarrow t)$ 一開始就已經 $\leq k$，那我們的方法數就直接輸出 $\dfrac{n(n - 1)}{2}$。否則，我們的想法就是找到滿足能從 $s \rightarrow u \rightarrow v \rightarrow t$ 的條件數，就是先用 Dijkstra 建出 $\text{dis}(s \rightarrow u)$ 與 $\text{dis}(t \rightarrow v)$，然後再想辦法用枚舉 $u$，$v$ 用二分搜之類的來計算。但此時會不會發生我們能從 $s \rightarrow u \rightarrow v \rightarrow t$ 且能從 $s \rightarrow v \rightarrow u \rightarrow t$ 的 $(u, v)$ 呢? 因為那會導致我們重複計算。我們來先試著列式一下，看這種情況存不存在：
    
        **<u>證明</u>**：從 $s \rightarrow u \rightarrow v \rightarrow t$ 或 $s \rightarrow v \rightarrow u \rightarrow t$ 皆可是存在的
        
        $$
        \begin{align}
        &\begin{cases}
        \text{dis}(s \rightarrow u) + \ell + \text{dis}(v \rightarrow t) \leq k \\
        \text{dis}(s \rightarrow v) + \ell + \text{dis}(u \rightarrow t) \leq k
        \end{cases}
        \\
        \Rightarrow&\begin{cases}
        \text{dis}(s \rightarrow u) + \text{dis}(v \rightarrow t) \leq k - \ell \\
        \text{dis}(s \rightarrow v) + \text{dis}(u \rightarrow t) \leq k - \ell
        \end{cases}
        \\
        \Rightarrow &\space \space\space\text{dis}(s \rightarrow u) + \text{dis}(v \rightarrow t) + \text{dis}(s \rightarrow v) + \text{dis}(u \rightarrow t) \leq 2k - 2\ell
        \end{align}
        $$
    
        由於 $\text{dis}(s \rightarrow t) > k$
    
        $$
        \begin{align}
        &\min\{\text{dis}(s \rightarrow u) + \text{dis}(u \rightarrow t), \text{dis}(s \rightarrow v) + \text{dis}(s \rightarrow t)\} > k
        \\
        \Rightarrow & \begin{cases}\text{dis}(s \rightarrow u) + \text{dis}(u \rightarrow t) > k \\ \text{dis}(s \rightarrow v) + \text{dis}(s \rightarrow t) > k\end{cases}
        \\
        \Rightarrow &\space \text{dis}(s \rightarrow u) + \text{dis}(u \rightarrow t) + \text{dis}(s \rightarrow v) + \text{dis}(v \rightarrow t) > 2k
        \\
        \end{align}
        $$
        
        代表 $2k\le 2k-2\ell$，這只會在 $\ell < 0$ 成立，但我們 $1\le \ell \le 10^9$，所以無法成立，代表結果矛盾的，不存在這種情況。所以我們就只需要算 $s \rightarrow u \rightarrow v \rightarrow t$ 的 $(u, v)$ 數量就好，完全不用考慮重複算的問題。這個有很多種作法，以下提供兩種：
    
        1. 可以先把 $\text{dis}(t \rightarrow v)$ sort 好，然後固定 $u$，去二分搜滿足條件的 $v$。
        2. 或是可以先把 $\text{dis}(s \rightarrow u),\text{dis}(t \rightarrow u)$ 都由小到大分別 sort 好，然後因為滿足的 pair 具有單調性，可以用 two pointer 算，具體見代碼。
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long
    
        using namespace std;
        using pii = pair<int, int>;
    
        const int N = 2e5 + 5;
        int n, m, s, t, l, k;
        int ans, ds[N], dt[N];
        bool vis[N];
        vector<pii> G[N];
    
        void dij(int dis[], int s) {
            priority_queue<pii, vector<pii>, greater<pii>> q;
            memset(vis, 0, sizeof(vis));
            dis[s] = 0;
            q.push({0, s});
    
            while (!q.empty()) {
                auto [sum, u] = q.top();
                q.pop();
                if (vis[u]) {
                    continue;
                }
                vis[u] = 1;
                for (auto [v, w] : G[u]) {
                    if (dis[v] > dis[u] + w) {
                        dis[v] = dis[u] + w;
                        q.push({dis[v], v});
                    }
                }
            }
        }
    
        signed main() {
            ios::sync_with_stdio(false);
            cin.tie(nullptr);
            cin >> n >> m >> s >> t >> l >> k;
            for (int i = 1; i <= m; ++i) {
                int u, v, w;
                cin >> u >> v >> w;
                G[u].push_back({v, w});
                G[v].push_back({u, w});
            }
    
            memset(ds, 0x3f, sizeof(ds));
            memset(dt, 0x3f, sizeof(dt));
            dij(ds, s), dij(dt, t);
    
            if (ds[t] <= k) {
                cout << n * (n - 1) / 2 << '\n';
                return 0;
            }
    
            sort(ds + 1, ds + 1 + n);
            sort(dt + 1, dt + 1 + n);
    
            int j = 0;
            for (int i = n; i >= 1; i--) {
                while (ds[i] + l + dt[j + 1] <= k && j < n) {
                    j++;
                }
                ans += j;
            }
            cout << ans << '\n';
            return 0;
        }
    	```

### 次短路

第二次跑到某個點的時候就代表那個點的次短路

<figure markdown>
  ![Image title](./images/142.png){ width="700" }
</figure>

??? note "次短路實作"
	```cpp linenums="1"
    void dijkstra (int s) {
        priority_queue<pii, vector<pii>, greater<pii>> pq; 
        pq.push({0, s});

        while (pq.size()) {
            auto [sum, u] = pq.top();
            pq.pop();
    
            if (ans[u].f == -1) {
            	ans[u].f = sum;
            } else if (ans[u].s == -1) {
            	if (sum == ans[u].f) continue;// 嚴格要加這行
            	ans[u].s = sum;
            } else {
            	continue;
            }
    
            for (auto [v, w] : G[u]) {
            	pq.push({sum + w, v});
            }
        }
    }
    ```

???+note "模板 [USACO 2006 NOV Roadblocks G](https://www.luogu.com.cn/problem/P2865)"
	給一張 n 點 m 邊帶權無向圖，問 n 到 1 的嚴格次短路
	
	$n\le 5000, m\le 10^5$

???+note "嚴格次短路方法數 [AcWing - 383.觀光](https://www.acwing.com/problem/content/385/)"
	給一張 $n$ 點 $m$ 邊有向帶權圖，求 $s\to t$ 的 :
	
	- 最短路方法數
	
	- 比最短路多一單位的方法數
	
	$N\le 1000,M \le 10^4,$ 有 $T$ 筆測資
	
	??? note "思路"
		$dp_f(v)=\sum \limits_{f(v)+w(v, u)==f(u)}dp_f(u)$
		
		$dp_g(v)=\sum \begin{cases}dp_g(u) & \text{ if } g(v) + w(v, u) == g(u) \\ dp_f(u) & \text{ if } f(v) + w(v, u) == g(u) \end{cases}$
	
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
	
	    struct Edge {
	        int u, v, w;
	    };
	
	    struct Graph {
	        int n, m, s, t;
	        vector<vector<Edge>> G;
	        vector<int> f, g;
	        vector<int> dp_f, dp_g;
	
	        Graph(int _n, int _m) {
	            n = _n, m = _m;
	            f = vector<int>(n, INF);
	            g = vector<int>(n, INF);
	            dp_f = vector<int>(n);
	            dp_g = vector<int>(n);
	            G.resize(n);
	        }
	
	        void add_edge(int u, int v, int w) {
	            G[u].pb({u, v, w});
	        }
	
	        void sec(int u, int x) {
	            if (f[u] < x && g[u] == INF)
	                g[u] = x;
	            else if (f[u] < x && x < g[u])
	                g[u] = x;
	        }
	
	        void dijkstra() {
	            priority_queue<pii, vector<pii>, greater<pii>> pq;
	            pq.push({0, s});
	
	            while (pq.size()) {
	                auto [x, u] = pq.top();
	                pq.pop();
	
	                if (f[u] != INF) continue;
	                f[u] = x;
	
	                for (auto [u, v, w] : G[u]) {
	                    pq.push({w + f[u], v});
	                }
	            }
	        }
	
	        int find_second_best() {
	            priority_queue<pii, vector<pii>, greater<pii>> pq;
	            vector<int> vis(n);
	            for (int i = 0; i < n; i++) {
	                for (auto [u, v, w] : G[i]) {
	                    sec(v, f[i] + w);
	                }
	            }
	
	            for (int i = 0; i < n; i++) {
	                pq.push({g[i], i});
	            }
	
	            while (pq.size()) {
	                auto [x, u] = pq.top();
	                pq.pop();
	
	                if (vis[u]) continue;
	                vis[u] = 1;
	
	                for (auto [u, v, w] : G[u]) {
	                    sec(v, x + w);
	                    pq.push({g[v], v});
	                }
	            }
	        }
	
	        void build_DAG(vector<int> &dis, vector<vector<Edge>> &D) {
	            for (int i = 0; i < n; i++) {
	                for (auto [u, v, w] : G[i]) {
	                    if (dis[u] + w == dis[v]) {
	                        D[u].pb({u, v, w});
	                    }
	                }
	            }
	        }
	
	        void topo(vector<int> &dp, vector<vector<Edge>> &D) {
	            vector<int> in(n);
	            for (int i = 0; i < n; i++) {
	                for (auto [u, v, w] : D[i]) {
	                    in[v]++;
	                }
	            }
	
	            queue<int> q;
	            for (int i = 0; i < n; i++) {
	                if (in[i] == 0) q.push(i);
	            }
	
	            while (q.size()) {
	                int u = q.front();
	                q.pop();
	
	                for (auto [u, v, w] : D[u]) {
	                    in[v]--;
	                    dp[v] += dp[u];
	                    if (in[v] == 0) q.push(v);
	                }
	            }
	        }
	
	        int solve() {
	            int res = 0;
	
	            dijkstra();
	            vector<vector<Edge>> Df(n);
	            build_DAG(f, Df);
	            dp_f[s] = 1;
	            topo(dp_f, Df);
	
	            res += dp_f[t];
	            find_second_best();
	            if (g[t] == INF || g[t] != f[t] + 1) return res;
	
	            vector<vector<Edge>> Dg(n);
	            vector<vector<Edge>> Dfg(n);
	
	            build_DAG(g, Dg);
	
	            // f[u] -> w -> g[v]
	            for (int i = 0; i < n; i++) {
	                for (auto [u, v, w] : G[i]) {
	                    if (f[u] + w == g[v]) {
	                        dp_g[v] += dp_f[u];
	                    }
	                }
	            }
	            topo(dp_g, Dg);
	
	            res += dp_g[t];
	            return res;
	        }
	    };
	
	    void work() {
	        int n, m, s, t;
	        cin >> n >> m;
	        Graph g(n, m);
	
	        int u, v, w;
	        for (int i = 0; i < m; i++) {
	            cin >> u >> v >> w;
	            u--, v--;
	            g.add_edge(u, v, w);
	        }
	        cin >> s >> t;
	        s--, t--;
	        g.s = s, g.t = t;
	
	        cout << g.solve() << "\n";
	    }
	
	    signed main() {
	        ios::sync_with_stdio(0);
	        cin.tie(0);
	        int t = 1;
	        cin >> t;
	        while (t--) {
	            work();
	        }
	    }
	    ```

???+note "[TIOJ 2204.交替路徑](https://tioj.ck.tp.edu.tw/contests/81/problems/2204)"
	給一張 $n$ 點 $m$ 邊的簡單無向圖，每一條邊有兩個權重長度 $w_i$，顏色 $c_i$。定義「交替路徑」為沒有**相鄰**兩條邊有相同顏色的路徑(不一定是簡單路徑)。求全點對最短「交替路徑」長
	
	$n \le 500, m \le \frac{n(n-1)}{2}$
	
	??? note "思路"
		因為只需考慮相鄰的邊，我們只要看結尾的顏色
		
		考慮 $i \to j$ 是一條最短交替路徑，現在我想要從 $j$ relax 周圍的點，我一定是拿最短的嘛！
	   
		除非某條邊 $j \to k$ 的顏色和 $i \to j$ 的結尾顏色一樣
		
		這個時候一定是拿「結尾顏色不一樣的次短交替路徑」
		
		所以只需要維護最短的與次短的，並確保結尾顏色不相同
		
		使用 $n^2$ dijkstra 實作，最短的與次短當成兩個不同的點來看
		
		詳見代碼
		
	??? note "code"
		```cpp linenums="1"
		#pragma GCC optimize("O3,unroll-loops")
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
	    const int mod2 = 5e8 + 4;
	    const int M = 1e9 + 7;
	
	    int n, m;
	
	    struct Edge {
	        int u, v, w, c;
	    };
	
	    struct triple {
	        int a, b, c;
	    };
	
	    struct Node {
	        int c1 = -1, c2 = -1, dis1 = INF, dis2 = INF, vis1, vis2;
	        // c1, dis1 : 當前最短交替路徑的顏色, 長度
	        // c1, dis1 : 當前與最短顏色不同的交替路徑的顏色, 長度
	        // vis1, vis2 : 是否已經固定 (被拿來 relax 起他人)
	        // c != -1, vis = 0 已入堆, 尚未固定
	    };
	
	    struct Graph {
	        vector<vector<Edge>> G;
	
	        void init() {
	            vector<vector<Edge>>().swap(G);
	            G.resize(n);
	        }
	
	        void add_edge(int u, int v, int w, int c) {
	            G[u].pb({u, v, w, c});
	            G[v].pb({v, u, w, c});
	        }
	
	        vector<int> dijkstra(int s) {
	            vector<Node> node(n);
	            node[s].c1 = 0;
	            node[s].dis1 = 0;
	
	            auto sec = [&](int u, int dis, int c) {
	                if (node[u].vis1 == 0) {
	                    if (dis < node[u].dis1) {
	                        if (c != node[u].c1) {
	                            node[u].dis2 = node[u].dis1;
	                            node[u].c2 = node[u].c1;
	                        }
	                        node[u].dis1 = dis;
	                        node[u].c1 = c;
	                        return;
	                    }
	                }
	
	                if (node[u].vis2 == 0) {
	                    if (dis < node[u].dis2) {
	                        if (c != node[u].c1) {
	                            node[u].dis2 = dis;
	                            node[u].c2 = c;
	                        }
	                    }
	                }
	            };
	            auto find = [&]() {
	                int u = -1, c, dis = INF, ord;
	                for (int i = 0; i < n; i++) {
	                    if (node[i].vis1 == 0 && node[i].c1 != -1) {
	                        if (node[i].dis1 < dis) {
	                            u = i, c = node[i].c1, dis = node[i].dis1;
	                            ord = 1;
	                        }
	                    }
	                    if (node[i].vis2 == 0 && node[i].c2 != -1) {
	                        if (node[i].dis2 < dis) {
	                            u = i, c = node[i].c2, dis = node[i].dis2;
	                            ord = 2;
	                        }
	                    }
	                }
	                if (u == -1) return (triple){-1, -1, -1};
	
	                if (ord == 1)
	                    node[u].vis1 = 1;
	                else
	                    node[u].vis2 = 1;
	
	                return (triple){u, dis, c};
	            };
	
	            for (int i = 1; i <= 2 * n - 1; i++) {
	                auto [u, dis, c] = find();
	                if (u == -1) break;
	
	                for (auto [u, v, ew, ec] : G[u]) {
	                    if (c != ec) sec(v, dis + ew, ec);
	                }
	            }
	
	            vector<int> dis(n);
	            for (int i = 0; i < n; i++) {
	                if (node[i].vis1 == 0)
	                    dis[i] = 0;
	                else
	                    dis[i] = node[i].dis1;
	            }
	
	            return dis;
	        }
	    } G;
	
	    void init() {
	        cin >> n >> m;
	
	        G.init();
	        int u, v, w, c;
	        for (int i = 0; i < m; i++) {
	            cin >> u >> v >> w >> c;
	            u--, v--;
	            G.add_edge(u, v, w, c);
	        }
	    }
	
	    void work() {
	        int ans = 0;
	        for (int i = 0; i < n; i++) {
	            vector<int> dis = G.dijkstra(i);
	            for (int j = 0; j < n; j++) {
	                ans = (ans + ((i + j + 2) * dis[j]) % M) % M;
	            }
	        }
	
	        cout << (ans * mod2) % M << "\n";
	    }
	
	    signed main() {
	        ios::sync_with_stdio(0);
	        cin.tie(0);
	        int t = 1;
	        cin >> t;
	        while (t--) {
	            init();
	            work();
	        }
	    }
	    ```

### K 短路

> dijkstra 正確性證明

> 你把狀態 $w$ 推出去的時候 狀態 $<w$ 都已經推出去了

> 所以當 $w$ 被推出去的時候 就保證是最佳解

延伸 : A*、 yen's algorithm

每個點跑進去 k 次即可

??? note "K 短路 code"
	```cpp linenums="1"
	void dijkstra(int s) {
        priority_queue<pii, vector<pii>, greater<pii>> pq; //{val,id}
        pq.push({0, s});

        while (pq.size()) {
            int sum = pq.top().f, u = pq.top().s;
            pq.pop();
    
            if (dis[u].size() >= k) continue;
            dis[u].pb(sum);
    
            for (auto [v, w] : G[u])
                pq.push({sum + w, v});
        }
    }
    ```

???+note "模板 [CSES - Flight Routes](https://cses.fi/problemset/task/1196)"
	給一張 $n$ 點 $m$ 邊的有向正權圖，$1$ 為起點，問起點到終點 $n$ 的前 $k$ 短路
	
	$n\le 10^5,m\le 2\times 10^5$

### 線段樹優化建圖

詳見此處

### 練習題

???+note "[CF 1051 F.The Shortest Statement](https://codeforces.com/problemset/problem/1051/F)"
	給一個 $n$ 點 $m$ 邊的無向圖，$q$ 筆詢問 :
	
	- $s_i\to t_i$ 的最短路徑
	
	$n,m,q\le 10^5,m-n\le 20$
	
	??? note "思路"
		觀察到 $m-n\le 20$
		
		若 $m=n-1$ 那就是一顆樹，我們可以將最短路分成全部都在樹上，跟有走到非樹邊的情況
		
		全部都在樹上 :
		
		樹上最短路，LCA
		
		有走到非樹邊 :
		
		因為這種邊最多只有 $21$ 條，也就是涵蓋 $42$ 個點，所以我們可以暴力以這 $42$ 個點為源點跑 dijkstra
		
		$s\to u \to t$，我們枚舉這 $42$ 個 $u$

???+note "[CF 715 B. Complete The Graph](https://codeforces.com/contest/715/problem/B)"
	給一張 $n$ 點 $m$ 邊無向圖，其中有一些邊要你指定權重
	
	求是否有方案使得從 $s\to t$ 的最短路恰為 $L$，輸出這些邊指定後的權重，或無法達成
	
	$n\le 1000,m\le 10^4,L\le 10^9$
	
	??? note "思路"
		接下來說的「邊」都指代「邊權未知的邊」。
	
	    將所有邊都設為 $L+1$，如果 $dis(s,t) < L$ ，那麼必然無解
	
	    將所有邊都設為 $1$ ，如果 $dis(s,t) > L$ ，那麼必然無解
	
	    考慮將任意一條邊的權值 $+1$，則 $dis(s,t)$ 會 $+0$ 或者 $+1$ 
	
	    如果將所有邊按照「隨便」一個順序不斷 $+1$，直到所有邊的權值都是 $10^9$ 了，那麼在這個過程中，$dis(s,t)$ 是遞增的，而且一定在某一個時刻 $dis(s,t)=L$
	
	    這樣的話我們就可以二分答案 + dijkstra解決這個問題了
	
	    時間複雜度 $\log (mL)\times m\log m = O(m\log m \log (mL))$ 
		
		---
		
		考慮邊權皆為 $1$ 的最短路，邊權皆為 INF 的最短路，有解若且唯若 $L$ 在這兩個值之間
		
		邊權皆為 $x$ 可得出最短路具有單調性
	
		小數點二分搜 $x$，將每個邊權都設為 $x$，使最短路比 $L$ 大一點點
		
		建立 shortest path DAG，將其中一條路徑向下取整，其他邊權即設為 INF
	
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
	
	    struct Edge {
	        int u, v, w, id;
	    };
	
	    struct Graph {
	        int run = 0;
	        int n, m, s, t, L;
	        vector<Edge> edges;
	        vector<int> dis;
	        vector<vector<pii>> G; // {w, v}
	
	        Graph (int n, int m, int s, int t, int L) : n(n), m(m), s(s), t(t), L(L) {}
	
	        void add_edge (int u, int v, int w, int id) {
	            edges.pb ({u, v, w, id});
	        }
	
	        int dijkstra () {
	            priority_queue<pii, vector<pii>, greater<pii>> pq;
	            pq.push ({0, s});
	
	            while (pq.size ()) {
	                auto [sum, u] = pq.top(); pq.pop();
	
	                if (dis[u] != INF) continue;
	                dis[u] = sum;
	
	                for (auto [w, v] : G[u]) {
	                    pq.push ({sum + w, v});
	                }
	            }
	            return dis[t];
	        }
	
	        int check (int x) {
	            //   0 1 2 3
	            //   4 5 6 7
	            // x = [m - 1, 10^9 * m - 1]
	            // 進行了 cnt=x/m 輪
	            // x %= m
	            // [0, x] +(cnt+1)
	            // [x + 1, m - 1] +(cnt)
	            G = vector<vector<pii>>(n, vector<pii>());
	            dis = vector<int>(n, INF);
	
	            int cnt = x / m;
	            x %= m;
	
	            for (auto [u, v, w, id] : edges) {
	                if (w == 0) {
	                    if (id <= x) {
	                        G[u].pb ({cnt + 1, v});
	                    } 
	                    else G[u].pb ({cnt, v});
	                }
	                else G[u].pb({w, v});
	            }
	
	            return dijkstra();
	        }
	    }; 
	
	    int n, m, L, s, t;
	
	    void work () {
	        cin >> n >> m >> L >> s >> t;
	        Graph g(n, m, s, t, L);
	
	        for (int i = 0; i < m; i++) {
	            int u, v, w;
	            cin >> u >> v >> w;
	            g.add_edge(u, v, w, i);
	            g.add_edge(v, u, w, i);
	        }
	
	        int l = m - 1, r = (1e9) * m - 1;
	        while (l < r) {
	            int mid = (l + r) / 2;
	
	            if (g.check(mid) < L) l = mid + 1;
	            else r = mid;
	        }
	        int dis = g.check (l);
	        if (dis != L) {
	            cout << "NO\n";
	            return;
	        }
	        cout << "YES\n";
	
	        map<pii, int> mp;
	        for (int i = 0; i < n; i++) {
	            for (auto [w, v] : g.G[i]) {
	                if (mp[{i, v}] || mp[{v, i}]) continue;
	                cout << i << " " << v << " " << w << "\n";
	                mp[{i, v}] = true;
	            }
	        }
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            work();
	        }
	    } 
	    ```

???+note "[全國賽模擬賽 2022 pD. 小風的遊戲 (Game)](https://www.csie.ntu.edu.tw/~b11902109/PreNHSPC2022/IqwxCSqc_Pre_NHSPC_zh_TW.pdf#page=9)"
	給一張 $n$ 點 $m$ 邊無向圖，目標是讓 $s$ 到 $t$ 的最短路徑長度恰好為 $d$。給一個 $1\ldots m$ 的 permutation $p$，代表 $w_{p_1}<w_{p_2} < \ldots < w_{p_m}$，問是否有辦法構造 $w_1, \ldots ,w_m$，有的話請輸出
	
	$n\le 10^5, m\le 2\times 10^5, 1\le d\le 10^{11}$
	
	??? note "思路"
		最小的解就是 $w_{p_1}=1,w_{p_2}=2,\ldots$，我們二分搜一個 threshold $t$，滿足使用邊權 $\le t$ 的邊恰能使 $s\to t$ 的最短路 $\le d$，若全部的邊都用上 $d$ 還是小於最短路 $L$，就輸出無解。
		
		接下來我們要來調整，讓最短路變成恰好 $d$。因為到 $w_i=t$ 才恰好形成 <= d 的最短路，所以 $w_i=t$ 一定在最短路徑上，而在這之前，最短路是 > d 的。如果我們將 $w_i$ 改成 $t+d-L$，可以使得最短路加起來恰好變成 $d$（因為沒有 $w_i$ 這條邊的路徑，權值一定 > d）。至於剩下的邊我們要使他們不會干預我們的最短路徑。$< t$ 的邊維持不變，因為上面的有最短路徑的圖就有涵蓋這些邊，也就是 $1, 2, \ldots$；$>t$ 的邊要保證不會出現在最短路徑上，就要設為 $d+1, d+2, \ldots$。
		
		<figure markdown>
          ![Image title](./images/23.png){ width="300" }
          <figcaption>d = 7</figcaption>
        </figure>

???+note "[CF 1307 D. Cow and Fields](https://codeforces.com/problemset/problem/1307/D)"
	給定一個 $n$ 個點 $m$ 邊無向圖，$n$ 個點中有 $k$ 個是特殊點，可以在這 $k$ 個點中找兩個點連一條無向邊。每條邊的距離都是 $1$。問從 $1$ 到 $n$ 的最短路最大是多少。
	
	$n,m,k\le 2\times 10^5$
	
	??? note "思路"
		現在有兩個點 $i$ 和 $j$ ，如果其建邊的話，最短路可能是 $1 \to i \to j \to n$ 或者 $1 \to j \to i \to n$。這樣代表的距離也就是 $dis(1\to i)+dis(j\to n)+1$ 和 $dis(1\to j)+dis(i\to n)+1$ 了。我們要取最小的，因此 $dis(1\to i)+dis(j\to n)+1<dis(1\to j)+dis(i\to n)+1$  時，才符合最短路的條件。移項後變為 $dis(1\to i) - dis(i\to n) < dis(1\to j)-dis(j\to n)$。依據 exchange argument，按照這個條件由小到大排序後，枚舉位於後面的點 $j$，然後找到點 $j$ 前面的 $dis(1\to i)$ 的最大值，這樣可以保證相加之和是最大的。最大就是之前的最短路了。最後與原圖最短路比較一下就可以了。

## Bellman Ford/SPFA

### Bellman Ford

Bellman-Ford 就是把所有節點都 relax，做 $n − 1$ 次，會對的原因是最短路徑最多只經過 $n − 1$ 條邊

???+note "模板 [CSES - Cycle Finding](https://cses.fi/problemset/task/1197)"
	給一張 $n$ 點 $m$ 邊有向圖，求上面是否有負環，如果有的話輸出任意負環
	
	$n \le 2500、m \le 5000$

??? note "算法實作"
	```cpp linenums="1"
	int x; // 看第 n 輪是否會 relax
	for (int i = 0; i < n; ++i) {
        x = -1; // 沒 relax
        for (auto &e: edges) {
            if (distances[e.v] > distances[e.u] + e.w) {
                distances[e.v] = distances[e.u] + e.w;
                parents[e.v] = e.u;
                x = e.v; // 有 relax
            }
        }
    }
    ```
	
??? note "code"
	```cpp linenums="1"
    #include <bits/stdc++.h>
    #define int long long
    using namespace std;

    struct Edge {
        int u, v, w;
    };

    int n, m;
    vector<Edge> edges;
    vector<int> distances;
    vector<int> parents;

    vector<int> construct_answer(int x) {
        for (int i = 0; i < n; ++i) {
            x = parents[x];
        }

        vector<int> ans;
        int y = x;
        do {
            ans.push_back(y);
            y = parents[y];
        } while (x != y);

        ans.push_back(x);
        reverse(ans.begin(), ans.end());

        return ans;
    }

    signed main() {
        cin >> n >> m;
        for (int i = 0; i < m; ++i) {
            int u, v, w;
            cin >> u >> v >> w;
            u--, v--;
            edges.push_back({u, v, w});
        }
        parents = vector<int>(n);
        distances = vector<int>(n);
        int x; // 看第 n 輪是否會 relax
        for (int i = 0; i < n; ++i) {
            x = -1; // 沒 relax
            for (auto &e: edges) {
                if (distances[e.v] > distances[e.u] + e.w) {
                    distances[e.v] = distances[e.u] + e.w;
                    parents[e.v] = e.u;
                    x = e.v; // 有 relax
                }
            }
        }
        if (x != -1) {
            auto ans = construct_answer(x);
            cout << "YES" << '\n';
            for (int i = 0; i < ans.size(); ++i) {
                cout << ans[i] + 1 << ' ';
            }
        } else {
            cout << "NO" << '\n';
        }
    }
    ```

### SPFA

#### 介紹

全名為 Shortest Path Finding Algorithm。屬於單源最短路，為 Bellman Ford 的優化版本，每回合只更新「前一回合有被鬆弛」的點相鄰的邊，實作上類似 dijkstra。如果要做很多次最短路，圖每次都變化，就可以用 SPFA，例如 MCMF，複雜度平均 $O(n+m)$，worst case $O(nm)$，含運氣成分

#### 概念(BFS)

如果上一輪某一個點的距離沒有更新,那這一輪也沒必要 relax 他。把距離有更新的節點丟進 queue 裡,然後一直拿 queue 裡的節點出來 relax。由於 BFS 處理環能力較弱，若遇到負環可能 TLE

??? note "SPFA BFS code"
	```cpp linenums="1"
	bool SPFA(int s) {
        vector<int> dis(n, INF);
        vector<bool> inq(n);
        vector<int> cnt(n);

        queue<int> q;
        q.push(s);
        dis[s] = 0;
        inq[s] = true;
    
        while (q.size()) {
            int u = q.front();
            q.pop();
            cnt[u]++;
    
            if (cnt[u] == n) {
                // negative cycle
                return true;
            }
    
            inq[u] = false;
    
            for (auto [v, w] : G[u]) {
    			if (dis[u] + w < dis[v]) {
                    dis[v] = dis[u] + w;
    
                    if (!inq[v]) {
                        inq[v] = true;
                        q.push(v);
                    }
                }
            }
        }
    
        return false;
    }
    ```

#### 概念(DFS)

如果一個 relax 操作是在 back edge 上進行的，則有負環。DFS 處理最短路能力較若弱，一般針對負環的題目[^1]。

若在判斷負環的題目時，會將 dis[ ] 初始值設為 0，使一開始正權的邊沒辦法走下去，減少額外的時間。

??? note "SPFA DFS code"
	```cpp linenums="1"
	int n, m;
    int dis[maxn];
    bool inq[maxn];
    vector<pii> G[maxn];

    bool spfa(int u) {
        inq[u] = true;
        for (auto [v, w] : G[u]) {
            if (dis[u] + w < dis[v]) {
                dis[v] = dis[u] + w;
                if (inq[v] || spfa(v)) {
                    return true;
                } 
            }
        }
        inq[u] = false;
        return false;
    } 
    
    bool check() {
        for (int i = 0; i < n; i++) {
            dis[i] = 0;
            inq[i] = false;
        }
    
        for (int i = 0; i < n; i++) {
            if (!inq[i]) {
                if (spfa(i)) return true;
            }
        }
        return false;
    }
    ```

### 題目

???+note "最小平均環 [LOJ #10084. 「一本通 3.3 练习 1」最小圈](https://loj.ac/p/10084)"
	給一張 $n$ 點 $m$ 邊有向圖，邊有權重，定義平均環為
	
	$$\mu(C)=\displaystyle \frac{\sum w_{u,v}}{|C|}$$
	
	求最小平均環 $\mu^*(C)=\min\{\mu(C) \}$
	
	$n\le 3000,m\le 10^4,|w_{i,j}|\le 10^7$
	
	??? note "思路"
		假設所求的平均最小值為 X，環上各個邊的權值分別為 A1,A2...Ak，可以得到 :
	
		X=(A1+A2+A3+...+Ak)/K
	
		A1+A2+A3+...+Ak=X*K
	
		移項可得：(A1-X)+(A2-X)+(A3-X)+...+(Ak-X)=0
	
		即判斷：(A1-ans)+(A2-ans)+(A3-ans)+...+(Ak-ans)<=0
	
		最後問題就變成了二分一個最大的 ans 滿足邊權為 w - ans 的圖不存在負環
		
		實作上需要使用 DFS SPFA，不然會 TLE
	
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
	
	    const double INF = 2e18;
	    const int maxn = 3000 + 5;
	    const int M = 1e9 + 7;
	    const double EPS = 1e-10;
	
	    int n, m;
	    double dis[maxn];
	    int vis[maxn];
	    vector<pair<int, double>> G[maxn];
	
	    bool spfa(int u, double t) {
	        vis[u] = true;
	        for (auto [v, w] : G[u]) {
	            w -= t;
	            if (dis[u] + w < dis[v]) {
	                dis[v] = dis[u] + w;
	                if (vis[v] || spfa(v, t)) {
	                    return true;
	                } 
	            }
	        }
	        vis[u] = false;
	        return false;
	    } 
	
	    bool check(double t) {
	        for (int i = 0; i < n; i++) {
	            dis[i] = 0;
	            vis[i] = false;
	        }
	
	        for (int i = 0; i < n; i++) {
	            if (!vis[i]) {
	                if (spfa(i, t)) return true;
	            }
	        }
	        return false;
	    }
	
	    signed main() {
	        ios::sync_with_stdio(0);
	        cin.tie(0);
	        cin >> n >> m;
	
	        for (int i = 0; i < m; i++) {
	            int u, v; double w;
	            cin >> u >> v >> w;
	            u--, v--;
	            G[u].pb({v, w});
	        }
	
	        double l = -1e7, r = 1e7;
	        while (r - l > EPS) {
	            double mid = (l + r) / 2;
	            if (check(mid)) r = mid;
	            else l = mid;
	        }
	        cout << fixed << setprecision(8) << l << '\n';
	    } 
		```

在看下面全國賽的題目前，我們先來看一道題目（與 Bellman-Ford 無關）

???+note "[LeetCode 134. Gas Station](https://leetcode.com/problems/gas-station/)"
	有兩個長度為 n 的環狀陣列，cost[i] 表示從 i<sup>th</sup> 到  (i+1)<sup>th</sup> 路上會消耗的汽油量，gas[i] 表示你站在 i<sup>th</sup> 可以得到的汽油量，選擇一個起點使得在走完一圈的過程中不能沒油
	
	??? note "思路"
	
		我們可以將 c[i] 表示為 gas[i] - cost[i]，要能繞完一圈的前提是 c[1]+...+c[n] >= 0
		
		> 這邊不懂可以看[這個影片](https://youtu.be/lJwbPZGo05A?t=100)
		
		假設起點為 k，令 suf[i] 為 c[i]+...+c[n]，那麼 k 是一個合法的起點若且唯若
		
		- i = k...n 這段不能為負
	
		- i = 1...(k - 1) 不能為負
	
		也就是可表示成
		
		- suf[k] - suf[i] >= 0
	
		- suf[k] + (suf[1] - suf[i]) >= 0
	
		顯然，suf[k] 越大越好，所以我們只需找 suf 最大的點即可
		
	??? note "code"
		```cpp linenums="1"
		class Solution {
	    public:
	        int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
	            int mx = -1e9, suf = 0, start = -1;
	            for (int i = gas.size()-1; i >= 0; i--) {
	                suf += gas[i] - cost[i];
	
	                if (suf > mx) {
	                    mx = suf;
	                    start = i;
	                }
	            }
	
	            return (suf >= 0) ? start : -1;
	        }
	    };
	    ```

從上面的題目我們可以觀察到以下性質

??? info "非負環必存在至少一個起點 $u$，過程中權重和都 $\ge 0$，且 suf 最大的必定滿足"
	
    非負環 $\Rightarrow$ suf[1] >= 0
    
    suf[k] = max (suf)
    
    因為 suf[k] 是最大的，所以 suf[k] - suf[i] 至少 >= 0
    
    - suf[k]+(suf[1] - suf[i]) >= 0
    
    - suf[k] - suf[i] >= 0

???+question "如何找非負環 ?"
	讓每個邊減一個數 $\epsilon$，使得正環 $1$ 還是正的，而零環可以變負環，那環上至多 $n$ 個點，若 $\epsilon = \frac{1}{n}$ 那就會使 $1\to 0$，而若減掉 $\epsilon = \frac{1}{n+1}$ 那就會使 $1$ 變成 $0.\cdots$ 還是正的，零環會變 $-0.\cdots$ 是負的

???+note "[全國賽 2021 pC](https://tioj.ck.tp.edu.tw/problems/2253)"
	給一張 $n$ 點（城市） $m$ 邊的有向圖 $G_0$。 我們對 $G_0$ 的每條邊都加上 $k$ 個點（村莊），得到一張 $n + mk$ 節點的有向圖 $G$，並賦予點權重 $c: V(G) \to Z$（每個節點的收支）。

	設 $C$ 是 $G$ 上的一個簡單環且 $u ∈ V(C)$。 若從 $u$ 出發沿著 $C$ 走一圈，任意前綴點權重和都 $\ge 0$，我們就說 $C$ 是 $G$ 的一個好環，而 $u$ 是 $C$ 的一個好起點。
	
	請找出 $G$ 的任一個好環 $C$ 與 $C$ 的任一個好起點 $u$，並求出 $C$ 上有幾個點可以當作好起點，這些好起點又有幾個在 $G_0$ 上。
	
	$k\le n\le 2000,m\le 8000$
	
	??? note "思路 (from twpca)"
		見 twpca

## Floyd warshall 

???+note "模板 [CSES - Shortest Routes II](https://cses.fi/problemset/task/1672)"
	給一張無向圖，$q$ 筆詢問求某兩點間的最短路徑
	
	$n \le 500,q \le 10^5$

??? note "算法實作"
	```cpp linenums="1"
	// init
	for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (i == j) dis[i][j] = 0;
            else if (adj[i][j] != INF) dis[i][j] = adj[i][j];
            else dis[i][j] = INF;
        }
    }
	
	// floyd warshall
	for (int k = 1; k <= n; k++) {
	    for (int i = 1; i <= n; i++) {
	        for (int j = 1; j <= n; j++) {
	            dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
	        }
	    }
	}
	```

### 最小環

???+note "[TIOJ  1212.圖論之最小圈測試](https://tioj.ck.tp.edu.tw/problems/1212)"
	給一張 $n$ 點 $m$ 邊有向圖，找一个最小權值和的環（Girth）
	
	$3\le n\le 500,m\le 10^5$

第一個想法是 Dijkstra，我們可以枚舉每條邊，移除該邊然後跑一次 dijkstra，更新此環的總和 $dis (u,v) + w$ 到答案，複雜度 $O(n^2\log n)$

第二個想法是 Floyd warshall，Floyd warshall 有個性質，在最外層迴圈 $k$ 開始時，$dis_{i,j}$ 僅考慮是 $[1,k)$ 的最短路，我們可以利用這性質讓環成為 $dis_{i,j}+w(i,k)+w(k,j)$，因為環上一定有一個節點編號最大的點，故正確性足夠。	

網路上有一個作法是直接將初始狀態 dis[i][i] 設為 INF，也可以 AC，但若圖改成無向圖（[洛谷 P6175 无向图的最小环问题](https://www.luogu.com.cn/problem/P6175)）就不能用了。但上面兩種做法依然可實用

??? note "實作"
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
    const int maxn = 500 + 5;
    const int M = 1e9 + 7;
    
    int n, m;
    int adj[maxn][maxn];
    int dis[maxn][maxn];
    
    int solve() {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i == j) dis[i][j] = 0;
                else if (adj[i][j] != INF) dis[i][j] = adj[i][j];
                else dis[i][j] = INF;
            }
        }
    
        int ans = INF;
        for (int k = 1; k <= n; k++) {
            for (int i = 1; i < k; i++) {
                for (int j = 1; j < k; j++) {
                    if (i != j) {
                        ans = min(ans, dis[i][j] + adj[j][k] + adj[k][i]);
                    }
                }
            }
    
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
                }
            }
        }
        if (ans == INF) return 0;
        return ans;
    }
    
    signed main() {
        while(cin >> n >> m) {
            if (n == 0 && m == 0) break;
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    adj[i][j] = INF;
                }
            }
            for (int i = 0; i < m; i++) {
                int u, v, w;
                cin >> u >> v;
                adj[u][v] = 1;
            }
            cout << solve() << '\n';
        }
    } 
    ```

???+note "[zerojudge b686. 6. 航線規劃](https://zerojudge.tw/ShowProblem?problemid=b686)"
	給一張 $n$ 點 $m$ 邊無向圖，邊帶權，每個點有一個權重 $a_i$
	
	有 $q$ 筆詢問，如下 :
	
	- $x,s,t:$ $A_i<x$ 的點都不能走，問 $s\to t$ 的最短路徑<br> 
	
	$n\le 500,m\le 10^5,q\le 2\times 10^5,1\le a_i,x,s,t\le n$
	
	??? note "思路"
		
	??? note "code"
		```cpp linenums="1"
		sort (A.rbegin(), A.rend()); // 防禦力大到小
	    sort (query.rbegin(), query.rend()); // 破壞力大到小
	
	    for (int q = 1; q <= query.size(); q++) {
	        int w = query[q];
	
	        // floyd 中繼點並非一次全部更新, 而是要得才更新
	        for (int k = 1; A[k] > w; k++) {
	           for (int i = 1; i <= n; i++) {
	               for (int j = 1; j <= n; j++) {
	                   d[i][j] = max (d[i][j], d[i][k] + d[k][j]);
	               }
	           } 
	        }
	    }
	    ```

???+note "[TIOJ 1034.搶救雷恩大兵 (Saving Ryan)](https://tioj.ck.tp.edu.tw/problems/1034)"
	給 $N\times N$ 的 grid，每個點都有權值
	
	$Q$ 筆詢問 :
	
	- 可以把一個點的權值改成 $0$ 的狀況下，$s_i\to t_i$ 的最短路最少是多少
	
	$N\le 20, Q\le N^4$
	
	??? note "思路"
		建表，對於每筆 query 枚舉中間點即可

???+note "[2021 一模 pA.挑選路徑(Shortcut)](https://drive.google.com/file/d/1iqPCeaaj49-CSO4BnjggvVCgDlsNt-dZ/view)"
	定義一張圖的總花費是所有點對之間的最短距離總和。給定一張 $n$ 點 $m$ 邊的簡單無向連通圖，在你可以加一條邊的情況下，和原圖相比最多可以減少多少總花費？又有幾種加邊的方式可以減少那麼多花費？
	
	$3 \leq n \leq 500,n-1 \leq m \leq \dfrac{n(n-1)}{2}-1$
	
	??? note "思路"
		【暴力作法: ${O}(n^5)$】

        ${O}(n^2)$ 枚舉要加的邊，每次重新做 $n$ 次 BFS（一次 ${O}(n+m)$ ）或是 Floyd-Warshall

        【列算式，預處理: ${O}(n^4)$】

        每次枚舉要加的邊 $(u,v)$ 時，計算每個點對的距離減少了多少，也就是 $\sum \limits_{i < j} \max\{0, \text{dis}(i,j) - (\text{dis}(i,u)+1+\text{dis}(v,j)), \text{dis}(i,j)-(\text{dis}(i,v)+1+\text{dis}(u,j))\}$，其中 $\text{dis}(i,j)$ 是原圖中 $i$ 與 $j$ 的最短距離，可以 ${O}(n^3)$ 預處理。

        【滿分解: ${O}(n^3)$】

        我們來證明看看是否從兩邊過來的路徑都存在（仿造 2024 JOI pB），假設 $\text{dis}(i\rightarrow j) = k$：

         **<u>證明</u>**：從 $i \rightarrow u \rightarrow v \rightarrow j$ 或 $i \rightarrow v \rightarrow u \rightarrow j$ 皆存在更短的路徑

        $$
        \begin{align}
        &\begin{cases}
        \text{dis}(i \rightarrow u) + \ell + \text{dis}(v \rightarrow j) \leq k \\
        \text{dis}(i \rightarrow v) + \ell + \text{dis}(u \rightarrow j) \leq k
        \end{cases}
        \\
        \Rightarrow&\begin{cases}
        \text{dis}(i \rightarrow u) + \text{dis}(v \rightarrow j) \leq k - \ell \\
        \text{dis}(i \rightarrow v) + \text{dis}(u \rightarrow j) \leq k - \ell
        \end{cases}
        \\
        \Rightarrow &\space \space\space\text{dis}(i \rightarrow u) + \text{dis}(v \rightarrow j) + \text{dis}(i \rightarrow v) + \text{dis}(u \rightarrow j) \leq 2k - 2\ell
        \end{align}
        $$

        而 $\text{dis}(i \rightarrow u) + \text{dis}(u \rightarrow j)$ 或 $\text{dis}(i \rightarrow v) + \text{dis}(v \rightarrow j)$ 至少都會大於等於 $k$（因為 $i$ 到 $j$ 的最短路是 $k$），假設是 $\text{dis}(i \rightarrow u) + \text{dis}(u \rightarrow j)\ge k$，這樣的話另外一條  $\text{dis}(i \rightarrow v) + \text{dis}(v \rightarrow j)$ 一定 $\le k-2\ell$，因為這題 $\ell =1$，我們發現最短路居然更短了，矛盾，所以只會存在一條。

        也就是說，我們在算總花費減少多少的時候可以改為計算 $\sum \limits_{i,j}\max\{ 0,\text{dis}(i,j)-(\text{dis}(i,u)+1+\text{dis}(v,j))\}$，注意這裡的 $(i,j)$ 是無序的。這種要枚舉好幾個變數的我們會想要試著枚舉其中幾個，另外一個使用類似資料結構優化。考慮固定 $i,v$，那麼 $j,u$ 之間是獨立的，我們可以拆成 $\max\{ 0,(\text{dis}(i,j)-\text{dis}(v,j)-1) - \text{dis}(i,u)\}$。問題就變成，對於每個 $a_u$，求 $\sum \limits_{j} \max\{ 0,b_j-a_u\}$ 的總和。這可以用 counting sort + two pointer 在線性時間內解決，具體來說，就是用一個 bucket 先預處理好對於每個數字 $t$，$b_j\ge t$ 的 $b_j$ 總和，與有幾個符合的 $b_j$，對於 $a_u$ 就是去看 $\text{sum}(t)-\text{cnt}(t)\cdot a_u$。總複雜度是枚舉所有 $i,v$ 所需的 ${O}(n^2)$，接著 $O(n)$ 枚舉 $j$ 去預處理 bucket，然後在預處理好後再 $O(n)$ 對於每個 $u$ 查表計算，所以整體複雜度就是 ${O}(n^2\cdot (n + n))=O(n^3)$。
        
		> 參考: <https://omeletwithoutegg.github.io/2021/09/22/toi-2021-sols/#p1-shortcut>
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long

        using namespace std;
        const int MAXN = 505, inf = 1e9;

        int dis[MAXN][MAXN];
        int ans[MAXN][MAXN];
        int sum[MAXN];
        int cnt[MAXN];
        int n, m;

        signed main() {
            ios_base::sync_with_stdio(0), cin.tie(0);
            cin >> n >> m;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (i != j) {
                        dis[i][j] = inf;
                    }
                }
            }
            for (int i = 0; i < m; i++) {
                int u, v;
                cin >> u >> v;
                u--, v--;
                dis[u][v] = dis[v][u] = 1;
            }
            for (int k = 0; k < n; k++) {
                for (int i = 0; i < n; i++) {
                    for (int j = 0; j < n; j++) {
                        dis[i][j] = min(dis[i][j], dis[i][k] + dis[k][j]);
                    }
                }
            }
            for (int i = 0; i < n; i++) {
                for (int v = 0; v < n; v++) {
                    for (int j = 0; j < n; j++) {
                        sum[j] = cnt[j] = 0;
                    }
                    for (int j = 0; j < n; j++) {
                        int d = dis[i][j] - dis[v][j] - 1;
                        if (d < 0) continue;
                        cnt[d] += 1;
                        sum[d] += d;
                    }
                    for (int j = n - 1; j >= 0; j--) {
                        sum[j] += sum[j + 1], cnt[j] += cnt[j + 1];
                    }
                    for (int u = 0; u < v; u++) {
                        int d = dis[i][u];
                        ans[u][v] += sum[d] - cnt[d] * d;
                    }
                }
            }

            int mx = -1;
            int cnt = 0;
            for (int i = 0; i < n; i++) {
                for (int j = i + 1; j < n; j++) {
                    if (ans[i][j] == mx) {
                        cnt++;
                    } else if (ans[i][j] > mx) {
                        mx = ans[i][j], cnt = 1;
                    }
                }
            }
            cout << cnt << ' ' << mx << '\n';
        }
        /*
        5 4
        1 2
        2 3
        3 4
        2 5

        5 5
        1 2
        2 3
        3 4
        4 5
        5 1
        */
        ```
	
---

## 參考資料

- <https://drive.google.com/file/d/1a1mgK8KFJWNoXATHwi3E6ceStn22QmZl/view>

- <https://drive.google.com/file/d/1q2mP9uHYAauroE2mjtYKti9khs0H9qaJ/view>

- <https://slides.com/peter940324/deck-f0e69c#/3/18>

- IOIC 2023

- [sprout 2023](https://www.csie.ntu.edu.tw/~sprout/algo2023/ppt_pdf/week12/graph1_inclass_tp.pdf)

[^1]: 這是筆者從[中國博客](https://blog.csdn.net/Hardict/article/details/82798409)抄來的，不知道實際上實不實用。