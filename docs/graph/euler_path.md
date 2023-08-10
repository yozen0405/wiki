- https://judge.ioicamp.org/contests/5/problems/511
	- 變化 : class 29

??? note "code"
	```cpp linenums="1"
	#pragma GCC optimize("O3,unroll-loops")
    #pragma GCC target("avx,popcnt,sse4,abm")
    #include <bits/stdc++.h>
    using namespace std;

    #ifdef WAIMAI
    #define debug(HEHE...) cout << "[" << #HEHE << "] : ", dout(HEHE)
    void dout() {cout << '\n';}
    template<typename T, typename...U>
    void dout(T t, U...u) {cout << t << (sizeof...(u) ? ", " : ""), dout(u...);}
    #else
    #define debug(...) 7122
    #endif

    //#define int long long
    #define ll long long
    #define Waimai ios::sync_with_stdio(false), cin.tie(0)
    #define FOR(x,a,b) for (int x = a, I = b; x <= I; x++)
    #define pb emplace_back
    #define F first
    #define S second

    const int SIZE = 4e5 + 5;

    int n, m, k;
    int d[SIZE];
    bool vs[SIZE];
    vector<int> adj[SIZE];

    void solve() {
        cin >> n >> m >> k;
        m -= k;
        FOR (i, 1, k) {
            int a, b;
            cin >> a >> b;
            d[a] ^= 1, d[b] ^= 1;
        }
        d[1] ^= 1, d[n] ^= 1;
        FOR (i, 1, m) {
            int a, b;
            cin >> a >> b;
            adj[a].pb(b);
            adj[b].pb(a);
        }
        FOR (i, 1, n) if (!vs[i]) {
            int cnt = 0;
            function<void(int)> dfs = [&](int pos) {
                vs[pos] = 1;
                cnt += d[pos];
                for (int np : adj[pos]) if (!vs[np]) dfs(np);
            };
            dfs(i);
            if (cnt & 1) {
                cout << "No\n";
                return;
            }
        }
        cout << "Yes\n";
    }

    int32_t main() {
        Waimai;
        int tt = 1;
        //cin >> tt;
        while (tt--) solve();
    }
    ```

- TIOJ 一筆畫
- [山姆的競程日記 - 2019 北市賽](https://sam571128.codes/2020/09/30/TIOJ-2019-Taipei-Contest/#%E7%AC%AC%E4%B8%89%E9%A1%8C%EF%BC%9A%E6%89%93%E5%8D%A1%E9%81%8A%E6%88%B2-Checkin)

- 中國郵差
	- https://drive.google.com/file/d/1k5vChKRoRRVM7KBxMl4vxdKg_qG1nCCP/view

- <https://blog.csdn.net/kdazhe/article/details/122160401>

- <https://drive.google.com/file/d/1q2mP9uHYAauroE2mjtYKti9khs0H9qaJ/view>

!!! info "歐拉迴路計數 - BEST 定理"

## 性質

### 有向圖

path: 起點 in = out + 1，終點 out = in + 1，其他 in=out

tour: for all in=out

### 無向圖

path: 起點, 終點 degree = odd 或 起點, 終點 degree = even

tour: for all degree = even

???+note "模板 [LOJ #10105. 「一本通 3.7 例 1」欧拉回路](https://loj.ac/p/10105)"
	給一張圖，找出歐拉迴路，即在圖中找一個環使得每條邊都在環上出現恰好一次，有兩個子任務
	
	- 這張圖是無向圖
	
	- 這張圖是有向圖
	
	$n\le 10^5,m\le 2\times 10^5$
	
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

        const int N = 1e6, M = 1e6;
        int t, n, m, in[N], out[N], ans[M], vis[M], top;
        vector<pii> G[N];

        void dfs(int u) {
            while(G[u].size()) {
                auto [v, id] = G[u].back();
                G[u].pop_back();

                if (vis[abs(id)]) continue;

                vis[abs(id)] = 1;
                dfs(v);
                ans[++top] = id;
            }
        }

        signed main() {
            cin >> t >> n >> m;

            for (int i = 1; i <= m; i++) {
                int u, v;
                cin >> u >> v;
                G[u].pb({v, i});

                if (t == 1) {
                    G[v].pb({u, -i});
                }

                out[u]++, in[v]++;
            }

            if (t == 1) {
                for (int i = 1; i <= n; i++) {
                    if ((in[i] + out[i]) % 2) {
                        cout << "NO\n";
                        return 0;
                    }
                }
            }

            if (t == 2) {
                for (int i = 1; i <= n; i++) {
                    if (in[i] != out[i]) {
                        cout << "NO\n";
                        return 0;
                    }
                }
            }

            for (int i = 1; i <= n; i++) {
                if (G[i].size()) {
                    dfs(i);
                    break;
                }
            }

            if (top != m) {
                cout << "NO\n";
                return 0;
            }

            cout << "YES\n";

            for (int i = top; i >= 1; i--) {
                cout << ans[i] << '\n';
            }
            return 0;
        }
        ```