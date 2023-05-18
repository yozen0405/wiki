- [LOJ 網路流 24 題](https://loj.ac/problems/tag/30)

## 模板
### dinic
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

const int INF = (1LL << 60);
const int M = 1e9 + 7;

int n;

struct dinic {
    struct Edge {
        int u, v;
        int cap;
    };

    int n, m, s, t;
    vector<vector<int>> G;
    vector<Edge> edges;
    vector<int> lv;
    vector<int> cur;
    vector<int> side;
    vector<int> ans;

    void init () {
        n = m = 0;
        G.clear ();
        edges.clear ();
    }

    int add_node () {
        n++;
        G.pb({});
        return n - 1;
    }

    void add_edge (int u, int v, int cap) {
        edges.pb({u, v, cap});
        G[u].pb(m++); // 0
        edges.pb({v, u, 0LL});
        G[v].pb(m++); // 1
    }

    void cut (int u) {
        side[u] = 1;
        ans.pb(u);
        for (auto i : G[u]) {
            if (!side[edges[i].v] && edges[i].cap > 0) {
                cut (edges[i].v);
            }
        }
    }

    bool bfs () {
        lv = vector<int> (n, -1);
        queue<int> q;
        lv[s] = 0;
        q.push (s);
        while (q.size()) {
            int u = q.front ();
            q.pop ();

            for (int i = 0; i < G[u].size(); i++) {
                Edge &e = edges[G[u][i]];
                if (e.cap > 0 && lv[e.v] < 0) {
                    lv[e.v] = lv[u] + 1;
                    q.push (e.v);
                }
            }
        }
        return lv[t] >= 0;
    }

    int dfs (int u, int f) {
        if (u == t || f == 0) return f;
        int res = 0;
        for (auto &i = cur[u]; i < G[u].size(); i++) {
            Edge &e = edges[G[u][i]];
            Edge &rev = edges[G[u][i] ^ 1];
            if (e.cap > 0 && lv[u] + 1 == lv[e.v]) {
                int x = dfs (e.v, min (f, e.cap));
                if (x > 0) {
                    e.cap -= x;
                    rev.cap += x;
                    f -= x;
                    res += x;
                    if (f == 0) break;
                }
            }
        }
        return res;
    }

    int max_flow (int _s, int _t) {
        s = _s, t = _t;
        int res = 0;
        while (bfs()) {
            cur = vector<int> (n, 0);
            while (true) {
                int f = dfs (s, INF);
                if (f == 0) break;
                res += f;
            }
        }
        return res;
    }

    void min_cut () {
        side = vector<int>(n, 0);
        cut (s);
        cout << ans.size() << "\n";
        for (auto ele : ans) cout << ele << "\n";
    }

    void print (int flow) {
        vector<Edge> ans;
        for (int i = 1; i < edges.size(); i += 2) {
            auto [u, v, cap] = edges[i];
            if (cap == 0) continue;
            ans.pb({u, v, cap});
            //cout << "u:" << v << ",v:" << u << ",cap:" << cap << "\n";
        }
        cout << n << " " << flow << " " << ans.size() << "\n";
        for (auto [u, v, cap] : ans) cout << v << " " << u << " " << cap << "\n";
    }
}flow;

void solve () {
    int n, m, s, t;
    cin >> n >> m >> s >> t;
    flow.init ();
    for (int i = 1; i <= n; i++) flow.add_node();
    for (int i = 0; i < m; i++) {
        int u, v, cap;
        cin >> u >> v >> cap;
        flow.add_edge(u, v, cap);
    }
    int f = flow.max_flow(s, t);
    flow.min_cut();

}
 
signed main() {
    // ios::sync_with_stdio(0);
    // cin.tie(0);
    int t = 1;
    // cin >> t;
    while (t--) {
        //init(); 
        solve();
    }
} 

```
### min cost max flow
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

const int INF = (1LL << 60);
const int M = 1e9 + 7;

int n;

struct dinic {
    struct Edge {
        int u, v, cap, c;
    };

    int n, m, s, t;
    vector<vector<int>> G;
    vector<Edge> edges;
    vector<int> lv;
    vector<int> cur;

    void init () {
        n = m = 0;
        G.clear ();
        edges.clear ();
    }

    int add_node () {
        n++;
        G.pb({});
        return n - 1;
    }

    void add_edge (int u, int v, int cap, int w) {
        edges.pb({u, v, cap, w});
        G[u].pb(m++); // 0
        edges.pb({v, u, 0LL, -w});
        G[v].pb(m++); // 1
    }

    pii flow (int _s, int _t) {
        s = _s, t = _t;
        int fl, cost;
        fl = cost = 0;
        int cnt = 0;
        while (true) {
            vector<int> dis = vector<int>(n, INF);
            vector<int> inq = vector<int>(n, 0);
            vector<int> pre = vector<int>(n, -1);
            vector<int> preL = vector<int>(n, -1);
            dis[s] = 0;
            queue<int> q;
            q.push(s);
            while (q.size()) {
                int u = q.front(); q.pop();
                inq[u] = 0;
                for ( int i = 0 ; i < (int)G[u].size() ; i++) {
                    int v = edges[G[u][i]].v;
                    int w = edges[G[u][i]].c;
                    int fw = edges[G[u][i]].cap;
                    
                    if (fw > 0 && dis[v] > dis[u] + w) {
                        pre[v] = u; preL[v] = G[u][i]; // bug: preL[v] = i;
                        dis[v] = dis[u] + w;
                        if (!inq[v]) {
                            inq[v] = 1;
                            q.push(v);
                        }
                    }
                }
            }

            if (dis[t] == INF) break;
            int tf = INF;
            int u, l;
            for (int v = t; v != s ; v = u ) {
                u = pre[v]; l = preL[v];
                tf = min(tf, edges[l].cap);
            }

            for (int v = t, u, l ; v != s ; v = u ) {
                u = pre[v]; l = preL[v];
                edges[l].cap -= tf;
                edges[l ^ 1].cap += tf;
            }

            cost += tf * dis[t];
            fl += tf;
        }
        return {fl, cost};
    }

}flow;

void solve () {
    int n, m, s, t;
    cin >> n >> m >> s >> t;
    flow.init ();
    for (int i = 1; i <= n; i++) flow.add_node();
    for (int i = 0; i < m; i++) {
        int u, v, cap, w;
        cin >> u >> v >> cap >> w;
        flow.add_edge(u, v, cap, w);
    }
    auto [f, cost] = flow.flow (s, t);
    cout << f << " " << cost << "\n";
}
 
signed main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int t = 1;
    // cin >> t;
    while (t--) {
        //init(); 
        solve();
    }
} 
```

## algo
![](https://i.imgur.com/9aKx784.png)


## min cut
### 理論
- 切下去的邊有從 $\texttt{s}$ 流向 $\texttt{t}$ 的 $\texttt{cap}$ 和
- $\texttt{max flow = min cut}$

![](https://i.imgur.com/sH47Epq.png)
![](https://i.imgur.com/ELXVA3m.png)

### 輸出一組 min cut
- 跑完 $\texttt{dinic}$ ， 跑下面這份 $\texttt{code}$ ，走 $s$ 往外走未流滿的 $\texttt{edge}$ 
- 枚舉邊，檢查每條邊 $u,v$  的狀況，若 $u$ 屬於 $s$ 而 $v$ 屬於 $t$ 即為所求



```cpp linenums="1"
bool side[MAXN];
void cut(int u) {
    side[u] = 1;
    for (int i : G[u]) {
        if (!side[edges[i].v] && edges[i].cap) {
            cut(edges[i].v);
        }
    }
}
```

## 二分圖
- capacity 都是 1
### 最小點覆蓋
#### 輸出答案
- 左邊跟 $\texttt{min cut}$ 同 $\texttt{t}$ 側的點
- 右邊跟 $\texttt{min cut}$ 同 $\texttt{s}$ 側的點

![](https://i.imgur.com/dAHG427.png)

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

const int INF = (1LL << 60);
const int M = 1e9 + 7;

int n;

struct dinic {
    struct Edge {
        int u, v;
        int cap;
    };

    int n, m, s, t;
    vector<vector<int>> G;
    vector<Edge> edges;
    vector<int> lv;
    vector<int> cur;
    vector<int> side;
    vector<int> ans;
    vector<int> tag;

    void init () {
        n = m = 0;
        G.clear ();
        edges.clear ();
    }

    int add_node () {
        n++;
        G.pb({});
        return n - 1;
    }

    void add_edge (int u, int v, int cap) {
        edges.pb({u, v, cap});
        G[u].pb(m++); // 0
        edges.pb({v, u, 0LL});
        G[v].pb(m++); // 1
    }

    void cut (int u) {
        side[u] = 1;
        tag[u] = 1;
        for (auto i : G[u]) {
            if (!side[edges[i].v] && edges[i].cap > 0) {
                cut (edges[i].v);
            }
        }
    }

    bool bfs () {
        lv = vector<int> (n, -1);
        queue<int> q;
        lv[s] = 0;
        q.push (s);
        while (q.size()) {
            int u = q.front ();
            q.pop ();

            for (int i = 0; i < G[u].size(); i++) {
                Edge &e = edges[G[u][i]];
                if (e.cap > 0 && lv[e.v] < 0) {
                    lv[e.v] = lv[u] + 1;
                    q.push (e.v);
                }
            }
        }
        return lv[t] >= 0;
    }

    int dfs (int u, int f) {
        if (u == t || f == 0) return f;
        int res = 0;
        for (auto &i = cur[u]; i < G[u].size(); i++) {
            Edge &e = edges[G[u][i]];
            Edge &rev = edges[G[u][i] ^ 1];
            if (e.cap > 0 && lv[u] + 1 == lv[e.v]) {
                int x = dfs (e.v, min (f, e.cap));
                if (x > 0) {
                    e.cap -= x;
                    rev.cap += x;
                    f -= x;
                    res += x;
                    if (f == 0) break;
                }
            }
        }
        return res;
    }

    int max_flow (int _s, int _t) {
        s = _s, t = _t;
        int res = 0;
        while (bfs()) {
            cur = vector<int> (n, 0);
            while (true) {
                int f = dfs (s, INF);
                if (f == 0) break;
                res += f;
            }
        }
        return res;
    }

    void min_cut () {
        side = vector<int>(n, 0);
        tag = vector<int> (n, 0);
        ans.clear ();
        cut (s);
        int N = n / 2 - 1;
        vector<pii> res;
        for (int i = 1; i <= N; i++) {
            if (tag[i] == 0) {
                res.pb({1, i});
            }
        }

        for (int i = N + 1; i <= 2 * N; i++) {
            if (tag[i] == 1) {
                res.pb({2, i - N});
            }
        }

        cout << res.size() << "\n";
        for (auto [F, S] : res) cout << F << " " << S << "\n";
    }
}flow;

void solve () {
    int n, m, s, t;
    // s = 0, t = 2n + 1
    // L = 1 ~ n
    // R = n + 1 ~ 2n
    cin >> n;

    flow.init ();

    s = 0, t = 2 * n + 1;
    for (int i = s; i <= t; i++) flow.add_node();
    // 加 s -> L 
    for (int i = 1; i <= n; i++) {
        flow.add_edge (s, i, 1);
    }   
    // 加 t -> R
    for (int i = n + 1; i <= 2 * n; i++) {
        flow.add_edge (i, t, 1);
    }

    // 記得 flow 是 0-base
    for (int i = 1; i <= n; i++) { // L
        for (int j = n + 1; j <= 2 * n; j++) { // R
            char x;
            cin >> x;
            if (x == 'o') flow.add_edge (i, j, 1);
        }
    }

    // L & t
    // R & s
    int f = flow.max_flow(s, t);
    flow.min_cut();

}
 
signed main() {
    // ios::sync_with_stdio(0);
    // cin.tie(0);
    int t = 1;
    // cin >> t;
    while (t--) {
        solve();
    }
} 
```
- https://cses.fi/problemset/task/1709/

### 最大獨立集
- 最大獨立集 $+$ 最小點覆蓋 $=n$
- $\begin{cases} 最大獨立集: 一個邊最多選一個 \\ 最小點覆蓋: 一個邊至少選一個 \end{cases}$
- $\texttt{proof}$
    - 一個邊上最多只有一個最大獨立集
    - 代表沒選的至少一個
    - 符合最小覆蓋定義

### 帶權最大獨立集
- 跑 $\texttt{min cost max flow}$ 即可

## DAG 最小點覆蓋
- 最少需要多少條路徑才可以蓋住所有的點，且任兩條路徑不能有共通的點
- 不重疊路徑數 $+$ 路徑長總和 $=n$
- 我想讓不重疊路徑數最小，就是想讓路徑常總和最小
- 路徑選好後每個點的 $\texttt{in degree}$ 跟 $\texttt{out degree}$ 最多就是 $\texttt{1}$
- 一個匹配就是一個路徑長 $+1$ ，我想讓路徑長越多越好，相當於要讓匹配最大
- $\Rightarrow$ 二分圖最大匹配

![](https://i.imgur.com/PBepEBG.png)

## 有向無向
- 不管什麼題型，無向從 $u \rightarrow v$ 和從 $v \rightarrow u$ 都是獨立的也就是你總共會看到 2 + 2 = 4 條
- 對於 $\texttt{min cut max flow}$ 輸出那些邊有用到
    - 有向: 對於 $m$ 條邊檢查
    - 無向: 他只會用其中一條邊，所以就看那兩條邊比較小的(流比較多出去)的反向邊
    - ![](https://i.imgur.com/T0n5E4z.png)

- 無向圖，對於一般 $\texttt{flow}$ 正反互相底消
    - ![](https://i.imgur.com/vODTOzt.png)