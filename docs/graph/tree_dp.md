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