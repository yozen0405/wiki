???+note "[TIOJ 2204.交替路徑](https://tioj.ck.tp.edu.tw/contests/81/problems/2204)"
	給一張 $n$ 點 $m$ 邊的簡單無向圖，每一條邊有兩個權重長度 $w_i$，顏色 $c_i$

	定義「交替路徑」為沒有「連續」兩條邊有相同的顏色的路徑
	
	求全點對最短「交替路徑」長
	
	$n \le 500, m \le \frac{n(n-1)}{2}$

解答 : <https://hackmd.io/@Dn7ABYK4QAyS02iuO5ydBw/rJvk_9oBY#/3>

submission : <https://tioj.ck.tp.edu.tw/submissions/345309>

???+note "code"
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
    };

    struct Graph {
        vector<vector<Edge>> G;

        void init () {
            vector<vector<Edge>>().swap (G);
            G.resize (n);
        }

        void add_edge (int u, int v, int w, int c) {
            G[u].pb ({u, v, w, c});
            G[v].pb ({v, u, w, c});
        }

        vector<int> dijkstra (int s) {
            vector<Node> node (n);
            node[s].c1 = 0; node[s].dis1 = 0;

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

                if (ord == 1) node[u].vis1 = 1;
                else node[u].vis2 = 1;

                return (triple){u, dis, c};
            };

            for (int i = 1; i <= 2 * n - 1; i++) {
                auto [u, dis, c] = find ();
                //cout << "u:" << u + 1 << ",c:" << c << ",dis:" << dis << "\n";
                if (u == -1) break;

                for (auto [u, v, ew, ec] : G[u]) {
                    //cout << "v:" << v + 1 << ",ec:" << ec << ",ew:" << ew << "\n";
                    if (c != ec) sec (v, dis + ew, ec);
                }
            }

            vector<int> dis (n);
            for (int i = 0; i < n; i++) {
                if (node[i].vis1 == 0) dis[i] = 0;
                else dis[i] = node[i].dis1;
            }

            return dis;
        } 
    } G;

    void init () {
        cin >> n >> m;

        G.init ();
        int u, v, w, c;
        for (int i = 0; i < m; i++) {
            cin >> u >> v >> w >> c;
            u--, v--;
            G.add_edge (u, v, w, c);
        }
    }

    void work () {
        int ans = 0;
        for (int i = 0; i < n; i++) {
            vector<int> dis = G.dijkstra (i);
            for (int j = 0; j < n; j++) {
                //cout << "i:" << i + 1 << ",j:" << j + 1 << ",dis:" << dis[j] << "\n";
                ans = (ans + ((i + j + 2) * dis[j]) % M) % M;
                //cout << dis[j] << " ";
            }
            //cout << "\n";
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