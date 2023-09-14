## 引入

???+note "[旅行推銷員問題(TSP)](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)"
	給一張 $n$ 點 $m$ 邊無向圖，邊帶權，求走過所有點的最短路徑
	
	$1\le n\le 16$

先把任意兩點的最短路徑 dis(i, j) 算出來，因為點可以重複走，所以我們將沒相鄰兩點的 dis(i, j) 拿來用也沒差。dis(i, j) 可用 Floyd Warshall 建出來。

$S_i$ 代表有沒有拜訪過第 $i$ 個城市，令 $dp(S, v)$ 為走過 $S$ 並且現在在 $v$ 的最短路徑。我們可以列出轉移式 : 

$$dp(S, v) = \min\{ dp(S -\{v\}, u) + w(u, v) \}$$

最後，答案就是 $\min\{ dp(\{ V \}, 0), dp(\{ V \}, 1),\ldots , dp(\{ V \}, n - 1\}$。這樣的時間複雜度是 $O(2^n \times n) \times O(n) = O(n^2 \times 2^n)$

??? note "code"
	```cpp linenums="1"
	#include <bits/stdc++.h>
    #define int long long
    #define pii pair<int, int>
    #define pb push_back
    using namespace std;

    const int INF = 0x3f3f3f3f;
    const int maxn = 21;
    vector<int> G[maxn];

    void shortestPathLength() {
        int n, eg;
        cin >> n >> eg;
        vector<vector<int>> d(n,vector<int>(n, INF));
        int u, v, w;
        for (int i = 0; i < eg; i++) {
            cin >> u >> v >> w;
            u--,v--;
            d[u][v] = w;
            d[v][u] = w;
            G[u].pb(v);
            G[v].pb(u);
        }
        vector<vector<int>> dp(n, vector<int>((1 << n), INF));
        int ans = INF;
        for(int mask = 0; mask < (1 << n); mask++){
            vector<int> b;
            for (int i = 0; i < maxn; i++) {
                if (mask & (1 << i)) {
                    b.pb(i);
                }
            }
            for(int i = 0;i < b.size(); i++){
                if(b.size() == 1 && b[0] == 0) { 
                    dp[0][mask] = 0;
                }
                for(int j = 0;j < b.size(); j++){
                    if(i == j) continue;
                    int v = b[i], u = b[j];
                    dp[v][mask] = min(dp[v][mask], dp[u][mask ^ (1 << v)] + d[v][u]);
                }
            }
        }
        int mask = (1 << n) - 1;
        for (int i = 1; i < n; i++) {
            ans = min(dp[i][mask] + d[i][0], ans);
        }
        if(ans >= INF) cout << -1 << "\n";
        else cout << ans << "\n";
    }

    signed main () {
        shortestPathLength();
    }
    ```

## 點只能走一次

???+note "[CSES - Hamiltonian Flights](https://cses.fi/problemset/task/1690)"
	給一張 $n$ 點 $m$ 邊有向圖，求走過所有點恰一次的方法數
	
	$2\le n\le 20,1\le m\le n^2$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        const int N = 20;
        const int M = 1000000007;

        vector<int> G[N];
        int dp[(1 << N)][N];

        signed main() {
            ios_base::sync_with_stdio(false);
            cin.tie(0);

            int n, m;
            cin >> n >> m;
            for (int i = 0; i < m; i++) {
                int u, v;
                cin >> u >> v;
                u--, v--;
                if (u == v) continue;
                G[v].push_back(u);
            }

            dp[1][0] = 1;
            for (int mask = 0; mask < (1 << n); mask++) {
                for (int i = 0; i < n; i++) {
                    if (mask & (1 << i)) {
                        for (int k : G[i]) {
                            dp[mask][i] += dp[mask ^ (1 << i)][k];
                        }
                        dp[mask][i] %= M;
                    }
                }
            }
            cout << dp[(1 << n) - 1][n - 1] << '\n';
        }
        ```

這時就不能去算沒相鄰的 i, j 兩點的 dis(i, j) 了，直接用輸入給的 adjacency matrix 即可

## 迴路

???+note "[Sprout OJ 187. 高棕梠觀光農場](https://neoj.sprout.tw/problem/187/)"
	給你一張 $n$ 點帶權完全圖，求最短的 Hamiltonian cycle 長。
	
固定起點，假設起點是 0，dp[{0}][1] = 0, dp[{v}][v] = INF，`ans = min{ dp[(1<<n) - 1][u]+ dis[0][u] }`

## 例題

???+note "[TOI 2022 pE. 間諜](https://tioj.ck.tp.edu.tw/problems/2250)"
	給一個 $n\times m$ 的 Grid，對於第 i 步，不能和第 i-1 步同行或同列或同對角線，輸出經過每個點恰好一次的 Hamiltonian cycle
	
	$n\times m\le 2000$