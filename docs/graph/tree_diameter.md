???+note "[CSES - Tree Diameter](https://cses.fi/problemset/task/1131/)"
	給一顆 $n$ 點的樹，求樹直徑
	

	$n\le 2\times 10^5$

- 樹直徑性質 (APCSC)

- 兩次 dfs 找樹直徑 - 正確性證明
	- code in CF 1085D 

??? note "兩次 dfs code"
	```cpp linenums="1"
	#include <bits/stdc++.h>
    using namespace std;

    const int MAXN = 2e5 + 5;
    vector<int> G[MAXN];
    
    int x = 0;
    int mx = 0;
    void dfs(int u, int par, int dist) {
        if (dist > mx) {
            mx = dist;
            x = u;
        }
        for (int v : G[u]) {
            if (v == par) continue;
            dfs(v, u, dist+1);
        }
    }
    
    void solve() {
        int n; cin >> n;
        for (int i=0; i<n-1; i++) {
            int u, v;
            cin >> u >> v;
            u--; v--;
            G[u].push_back(v);
            G[v].push_back(u);
        }
    
        dfs(0, -1, 0);
        mx = 0;
        dfs(x, -1, 0);
    
        cout << mx << endl;
    }
    
    int main() {
        ios::sync_with_stdio(0);
        cin.tie(0);
        int t = 1;
        while (t--) {
            solve();
        }
    }
    ```

??? note "dp code"
	```cpp linenums="1"
	#include <bits/stdc++.h>
    #define int long long
    using namespace std;

    const int maxn = 1e6;
    
    int n, ans = 0, h[maxn], dp[maxn];
    vector<int> G[maxn];
    
    void dfs(int u, int par) {
        int mx = 0, sec = 0;
        for (auto v : G[u]) {
            if (v == par) continue;
            dfs(v, u);
    
            if (dp[v] > mx) {
                sec = mx;
                mx = dp[v];
            } else if (dp[v] > sec) {
                sec = dp[v];
            }
        }
        dp[u] = mx + 1;
        ans = max(ans, mx + sec);
    }
    
    signed main() {
        cin >> n;
        int u, v;
        for (int i = 0; i < n - 1; i++) {
            cin >> u >> v;
            G[u].push_back(v);
            G[v].push_back(u);
        }
        dfs(1, 1);
        cout << ans << "\n";
    }
    ```

!!! warning "若邊權有負，要用樹 dp，不能用兩次 DFS"

### 題目

這篇 [CF Blog](https://codeforces.com/blog/entry/101271) 上面的

???+note "[CF 1085 D.A Wide, Wide Graph](https://codeforces.com/contest/1805/problem/D)"
	給一顆有 $n$ 個點的樹
	
	定義 $G_k$ 為 : 有幾個點 $u$ 至少存在一個點與他距離 $\ge k$ 
	
	依序求 $G_1,\ldots,G_n$
	
	$n\le 10^5$
	
	??? note "思路"
		我們只要求出對於每個點，與他距離最遠的點的距離即可
		
	??? note "code"
		```cpp linenums="1"
	    #include <bits/stdc++.h>
	    #define int long long
	    #define s second
	    #define f first
	    #define pii pair<int,int>
	    using namespace std;
	
	    const int INF = 0x3f3f3f3f, MAXN = 1e5 + 5, M = 1e9 + 7;
	    int t, n, m, dis[MAXN], mx[MAXN];
	    vector<vector<int>> G(MAXN);
	
	    int dfs(int u, int p, int d) {
	        int ans = u;
	        dis[u] = max (d, dis[u]);
	
	        for (auto v : G[u]) {
	            if (v != p) {
	                int tmp = dfs(v, u, d + 1);
	
	                if (dis[tmp] > dis[ans]) {
	                    ans = tmp;
	                }
	            }
	        }
	
	        return ans;
	    }
	
	    signed main() {
	        ios_base::sync_with_stdio(0);
	        cin.tie(0);
	        t = 1;
	
	        while (t--) {
	            cin >> n;
	
	            for (int i = 0, u, v; i < n - 1; i++) {
	                cin >> u >> v;
	                G[u].push_back(v);
	                G[v].push_back(u);
	            }
	
	            int s = dfs(1, 0, 0);
	            int t = dfs(s, 0, 0);
	            dfs(t, 0, 0);
	
	            sort(dis + 1, dis + 1 + n);
	            int x = 0;
	
	            for (int i = 1; i <= n; i++) {
	                while (x <= n && dis[x] < i) {
	                    x++;
	                }
	
	                if (x > n)
	                    cout << n << " ";
	                else
	                    cout << x << " ";
	            }
	        }
	    }
	    ```

???+note "[2020 TOI pB.建設人工島](https://tioj.ck.tp.edu.tw/problems/2189)"
	給 $n$ 個點，邊有權重的樹，求嚴格次長樹直徑
	
	$n\le 10^5$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	    #define pb push_back
	    #define mk make_pair
	    #define F first
	    #define S second
	    using namespace std;
	
	    const int INF = 2e18;
	    const int maxn = 1e5 + 5;
	    const int M = 1e9 + 7;
	
	    struct Eg {
	        int v, w;
	    };
	
	    struct node {
	        int mx, sec;
	    };
	
	    int n, m;
	    vector<Eg> G[maxn];
	    vector<node> dp(maxn);
	    int vis[maxn];
	    node ans;
	
	    void cal (int val, node &x) {
	        if (x.mx < val) x.sec = x.mx, x.mx = val;
	        else if (val != x.mx && x.sec < val) x.sec = val;
	    }
	
	    void dfs (int u) {
	        vis[u] = true;
	        for (auto [v, w] : G[u]) {  
	            if (vis[v] == 1) continue;
	
	            dfs (v);
	            // ans 的轉移式
	            cal (dp[v].mx + dp[u].mx + w, ans);
	            cal (dp[v].mx + dp[u].sec + w, ans);
	            cal (dp[u].mx + dp[v].sec + w, ans);
	            // 沒有 u.sec + v.sec 因為她一定會比其他的小(成為第三名)
	
	            // 把 v 的鏈更新 u.mx, u.sec
	            cal (dp[v].mx + w, dp[u]);
	            cal (dp[v].sec + w, dp[u]);
	        }
	    }
	
	    void init() {
	        cin >> n;
	        int u, v, w;
	        for (int i = 1; i <= n - 1; i++) {
	            cin >> u >> v >> w;
	            u++, v++;
	            G[u].pb({v, w});
	            G[v].pb({u, w});
	        }
	    }
	
	    void solve() {
	        dfs (1);
	        cout << ans.sec << "\n";
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
	    
???+note "[CF 1294 F. Tree Paths on a Tree](https://codeforces.com/problemset/problem/1294/F)"
	給你一顆 $n$ 個點的樹，選相異三個點 $a, b, c$，使得至少有包含 $a,b,c$ 其中兩個的 path 數量最大，輸出最大 path 數量和 $a,b,c$。
	
	$3\le n\le 2\times 10^5$
	
	??? note "思路"
		其中兩點一定是樹直徑的兩端點。剩下一個點如何確定？到兩個點距離合最大即可，在兩次 bfs 過程中保留兩個端點到任意點的距離然後找這個最大值就好
		
		或者也可以找到樹直徑兩端 s, t，將樹用 s 重新定根，求每個點 u 與 LCA(u, t) 的距離即可
		
		> 參考自 : <https://newcoder-glm.blog.csdn.net/article/details/104080663?ydreferer=aHR0cHM6Ly9ibG9nLmNzZG4ubmV0Lw%3D%3D>