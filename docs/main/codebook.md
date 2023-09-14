# codebook

## 思路
### 時間
- 上午場：5題/3hr
- 下午場: 3題/2hr
### 策略
- 1~1.5hr 前三題
- 每題先花五分鐘，讀清楚題目，看哪些子題是直接會做的 可以直接寫掉的題目都解完，再一題一題花多一些時間想
- 看完題目試著用自己的話簡化題目，小心驗證有沒有簡化錯
- 20 min -> 跳
- 先想過，放在腦袋裡，寫別的題目的時候，其實腦袋的潛意識也會一邊思考
- 阿題目如果意思要問的也要早點問（先看完題目的好處
- 有些不會的話，可以晚點比較仔細想過，再看想到什麼子題就寫什麼子題
- 先想簡單的子題目的，一方面避免漏拿簡單暴力的分數，一方面可以幫助釐清題目
- 分好幾輪第一輪拿一些會的子題第二輪仔細想過
- 那我先把全部的題目都看完再來拿子題

### DP
- 換一種狀態定義
- 從前往後轉移
- 正着做不想，那反這做

## 模板
```cpp
#pragma GCC optimize("O3", "unroll-loops")

unsigned seed = chrono::steady_clock::now().time_since_epoch().count();
mt19937_64 rng(seed);
uniform_int_distribution<int> dis(1,1000000);

long long RandomNumber(){
    return dis(rng);
}
```

## flow
### 模板
#### dinic
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
#### min cost max flow
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

### algo
![](https://i.imgur.com/9aKx784.png)


### min cut
#### 理論
- 切下去的邊有從 $\texttt{s}$ 流向 $\texttt{t}$ 的 $\texttt{cap}$ 和
- $\texttt{max flow = min cut}$

![](https://i.imgur.com/sH47Epq.png)

#### 輸出一組 min cut
- 跑完 $\texttt{dinic}$ ， 跑下面這份 $\texttt{code}$ ，走 $s$ 往外走未流滿的 $\texttt{edge}$ 
- 枚舉邊，檢查每條邊 $u,v$  的狀況，若 $u$ 屬於 $s$ 而 $v$ 屬於 $t$ 即為所求

```cpp
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

### 二分圖
- capacity 都是 1
#### 最小點覆蓋
##### 輸出答案
- 左邊跟 $\texttt{min cut}$ 同 $\texttt{t}$ 側的點
- 右邊跟 $\texttt{min cut}$ 同 $\texttt{s}$ 側的點

![](https://i.imgur.com/dAHG427.png)

#### 最大獨立集
- 最大獨立集 $+$ 最小點覆蓋 $=n$
- $\begin{cases} 最大獨立集: 一個邊最多選一個 \\ 最小點覆蓋: 一個邊至少選一個 \end{cases}$
- $\texttt{proof}$
    - 一個邊上最多只有一個最大獨立集
    - 代表沒選的至少一個
    - 符合最小覆蓋定義

#### 帶權最大獨立集
- 跑 $\texttt{min cost max flow}$ 即可

### DAG 最小點覆蓋
- 最少需要多少條路徑才可以蓋住所有的點，且任兩條路徑不能有共通的點
- 不重疊路徑數 $+$ 路徑長總和 $=n$
- 我想讓不重疊路徑數最小，就是想讓路徑常總和最小
- 路徑選好後每個點的 $\texttt{in degree}$ 跟 $\texttt{out degree}$ 最多就是 $\texttt{1}$
- 一個匹配就是一個路徑長 $+1$ ，我想讓路徑長越多越好，相當於要讓匹配最大
- $\Rightarrow$ 二分圖最大匹配

![](https://i.imgur.com/PBepEBG.png)

### 有向無向
- 不管什麼題型，無向從 $u \rightarrow v$ 和從 $v \rightarrow u$ 都是獨立的也就是你總共會看到 2 + 2 = 4 條
- 對於 $\texttt{min cut max flow}$ 輸出那些邊有用到
    - 有向: 對於 $m$ 條邊檢查
    - 無向: 他只會用其中一條邊，所以就看那兩條邊比較小的(流比較多出去)的反向邊
    - ![](https://i.imgur.com/T0n5E4z.png)

- 無向圖，對於一般 $\texttt{flow}$ 正反互相底消
    - ![](https://i.imgur.com/vODTOzt.png)



## pbds
```cpp
#include <bits/stdc++.h>
#include <bits/extc++.h>
using namespace __gnu_pbds;
using namespace std;

template <typename T>
using pbds = tree<T, null_type, std::less<T>, rb_tree_tag, tree_order_statistics_node_update>;


signed main () {
    pbds<int> st;
    st.insert(3);
    st.insert(7);
    int k = 2;
    cout << *st.find_by_order(k - 1) << "\n";
    // 0-base 回傳排名第 k 的元素, 不存在則 return st.end()
    cout << st.order_of_key(7) << "\n";
    // strictly 比 7 小的元素有幾個
    st.erase(7);
}
```
## hash

```cpp
#include <bits/stdc++.h>
#define int long long
#define pii pair<int, int>
#define pb push_back
#define mk make_pair
#define F first
#define S second
using namespace std;
 
const int INF = 2e18;
const int maxn = 1e6 + 5;
const int M = 1e9 + 7;
const int X = 131;

int n, m;
int x[3][maxn], inv[3][maxn], h[3][maxn], pre[3][maxn];
string s, t;

int fastpow (int a, int b) {
    int ret = 1;
    while (b != 0) {
        if (b & 1) ret = ret * a % M;
        a = a * a % M;
        b >>= 1;
    }
    return ret;
}

void build (string &s, int p) {
    int n = s.size();
    x[p][0] = 1;
    for (int i = 1; i < n; i++) {
        x[p][i] = x[p][i - 1] * X % M;
        inv[p][i] = fastpow (x[p][i], M - 2);
    }

    for (int i = 0; i < n; i++) {
        h[p][i] = (s[i] - 'a' + 1) * x[p][i] % M;
        pre[p][i] = (pre[p][i - 1] + h[p][i]) % M;
    }
}

int query (int l, int r, int p) {
    if (l == 0) return pre[p][r];
    return (pre[p][r] - pre[p][l - 1] + M) % M * inv[p][l] % M;
}

void init() {
    cin >> s >> t;
    n = s.size(), m = t.size();
}

void solve() {
    build (s, 1);
    build (t, 2);

    int cnt = 0;
    for (int i = 0; i + m - 1 < n; i++) {
        int l = i, r = i + m - 1;
        if (query (l, r, 1) == query (0, m - 1, 2)) {
            cnt++;
        }
    }

    cout << cnt << "\n";
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

## 大數
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

const int INF = (1LL << 60);
const int maxn = 3e5 + 5;
const int M = 1e9 + 7;

void initial (string &a, string &b) {
    while (a.size() < b.size()) a = '0' + a;
    while (b.size() < a.size()) b = '0' + b;
}

bool findMax (string &a, string &b) {
    if (a < b) {
        swap (a, b);
        return true;
    }
    return false;
}

bool del (string &a) {
    if (a[0] == '0') {
        a.erase (0, 1);
        return true;
    }
    return false;
}

void DelAllZero (string &a) {
    while (del(a)) {
        del (a);
    }
}

string add (string a, string b) {
    initial (a, b);
    a = '0' + a;
    b = '0' + b;
    for (int i = a.size() - 1; i >= 0; i--) {
        int num1 = a[i] - '0';
        int num2 = b[i] - '0';
        if (num1 + num2 > 9) {
            a[i - 1] = a[i - 1] - '0' + 1 + '0';
            a[i] = (num1 + num2 - 10)  + '0';
        }
        else a[i] = (num1 + num2) + '0';
    }
    del (a);
    return a;
}

string sub (string a, string b) {
    initial (a, b);
    if (a == b) return "0";
    int fg = findMax (a, b);
    for (int i = a.size() - 1; i >= 0; i--) {
        int num1 = a[i] - '0';
        int num2 = b[i] - '0';
        if (num1 < num2) {
            a[i - 1] = a[i - 1] - '0' - 1 + '0';
            a[i] = (num1 - num2 + 10)  + '0';
        }
        else a[i] = (num1 - num2) + '0';
    }
    DelAllZero (a);
    if (fg) return "-" + a;
    return a;
}

string mul (string a, string b) {
    string res = "0";
    initial (a, b);
    findMax (a, b);
    DelAllZero (b);

    for (int i = b.size() - 1; i >= 0; i--) {
        int num1 = b[i] - '0';
        if (i != b.size() - 1) a = a + '0';
        for (int i = 0; i < num1; i++) {
            res = add (a, res);
        }
    }

    DelAllZero (res);
    return res;
}

string div (string a, string b) {
    initial (a, b);
    if (a < b) return "0";
    DelAllZero (b);
    string res = "0";
    string restmp = "1";
    string tmp = b;
    for (int i = 0; i < (a.size() - b.size()); i++) {
        restmp += '0';
        tmp += '0';
    }

    initial (a, b);
    while (a >= b) {
        initial (a, tmp);
        if (a >= tmp) {
            a = sub (a, tmp);
            res = add (res, restmp);
        }
        else {
            restmp.erase (restmp.size() - 1);
            tmp.erase (tmp.size() - 1);
        }
        initial (a, b);
    }

    DelAllZero (res);
    return res;
}

void run (string &op, string &a, string &b) {
    if (op == "/") cout << div (a, b) << "\n";
    if (op == "*") cout << mul (a, b) << "\n";
    if (op == "+") cout << add (a, b) << "\n";
    if (op == "-") cout << sub (a, b) << "\n";
}

// void init () {
    
// }

void solve () {
    string a, b, op;
    cin >> a >> op >> b;
    run (op, a, b);
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

## 組合
### n 球 m 箱
#### 球異箱異
##### 有空箱
- $m \times m \times ..=m^n$
##### 沒空箱
- 排容原理
- 全 $-$ 一箱空 $+$ 二箱空 $+...+$ $m$ 箱空
- $m^n-C^{m}_{1} \times (m-1)^{n}+C^{m}_{2} \times (m-2)^{n}+..+C^{m}_{m} \times (m-m)^{n}$
```cpp
int F (int n, int m) {
    int ret = 0;
    for (int i = 0; i <= m; i++) {
        ret += ((i & 1) ? -1 : 1) * fastpow(m - i, n, M) * C(m, i) % M;
        ret = (ret % M + M) % M;
    }
    return ret;
}
```

#### 球同箱同
- 等同於算有幾個長度為 $m$ 的非遞減數列
- $\texttt{dp[i][j] = i}$ 項總合為 $\texttt{j}$ 的方法數
- $dp[i][j]=dp[i-1][j-1]+dp[i][j-i]$ 

#### 球同箱異
- 隔板法
- $C^{n+m-1}_{m-1}$

#### 球異箱同
- $\mathtt{DP}$
- $\texttt{dp[i][j] i 球 j 組}$
- $dp[i][j]=dp[i-1][j-1]+j\times dp[i-1][j]$

### 環上色

- 將 $n$ 個點鏈塗上 $k$ 種顏色，相同顏色的兩個點至少要間隔 $m$ 個節點，求出方法數

#### m=1

- $\texttt{dp[i][0/1]}$ 跟第一個是不同顏色/相同
- $dp[i][0]=dp[i-1][0] \times (k-2) + dp[i-1][1] \times (k-1)$
- $dp[i][1]=dp[i-1][0]$
- $\texttt{init: dp[1][1]=k}$
- $\texttt{ans: dp[n][0]}$
#### m=3

- $\texttt{dp(i, s):}$  考慮前 $\texttt{i}$ 的東西，$\texttt{s}$ 最後三個分別有沒有跟第一個一樣顏色
- $\texttt{s}$ 可能是 $\texttt{000, 001, 010, 100}$
- $dp[i][000]=dp[i-1][100] \times (k-3) + dp[i-1][000] \times (k-4)$
- $dp[i][001]=dp[i-1][000]$
- $dp[i][010]=dp[i-1][001] \times (k-3)$
- $dp[i][100]=dp[i-1][010] \times (k-3)$
- $\texttt{init: dp[3][100] = k * (k - 1) * (k - 2)}$
- $\texttt{ans: dp[n][000]}$

```cpp
pii ext_gcd (int a, int b) {
    if (b == 0) {
        return {1, 0};
    }
    pii p = ext_gcd(b, a % b);
    return {p.second, p.first - (a / b) * p.second};
}

int inv (int a, int m) {
    pii p = ext_gcd(a, m);
    return (p.first % m + m) % m;
}

int fastpow (int a, int b, int m) {
    int ret = 1;
    while (b != 0) {
        if (b & 1) ret = ret * a % m;
        a = a * a % m;
        b >>= 1;
    }
    return ret;
}

int prei[maxn], pinv[maxn], pref[maxn];

void build () {
    prei[0] = prei[1] = pinv[0] = pinv[1] = pref[0] = pref[1] = 1;
    for (int i = 2; i < maxn; i++) {
        pref[i] = pref[i - 1] * i % M;
        pinv[i] = (M - (M/i) * pinv[M % i] % M) % M;
        prei[i] = prei[i - 1] * pinv[i] % M;
    }
} 

int C (int n, int k) {
    return pref[n] * prei[k] % M * prei[n - k] % M;
}

int F (int n, int m) {
    int ret = 0;
    for (int i = 0; i <= m; i++) {
        ret += ((i & 1) ? -1 : 1) * fastpow(m - i, n, M) * C(m, i) % M;
        ret = (ret % M + M) % M;
    }
    return ret;
}
```

## 動態維護中位數
- 開兩個 `priority_queue` ，將輸入的數字分別分成大小兩堆 `p1` ， `p2`
- `p1` (前半段小的數字)是由大到小， `p2` (後半段數字)是由小到大
這樣取 `p1.top()` 和 `p2.top()` 在偶數個情況就可以取中位數平均
- 每次一有數字進來就直接進取 `p2` ，在基數個情況 `p1` 就會比 `p2` 少 $1$ 個
-  調整 `p1` `p2` 的 `size` 讓 `p1.top()` 或 `p2.top()` 能取到目前更新後的中位數
- 在基數個情況只要取 `p2.top()` 就是中位數




### sample input

第一行輸入一個正整數 $N$ ，代表數列的大小
第二行輸入 $N$ 個數字
```
6
1 7 4 2 5 9
```
### sample output

輸出中位數
```
1 4 4 3 4 4.5
```
### 虛擬碼
```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;
int n;
priority_queue<int> p1;
priority_queue<int,vector<int>,greater<int>> p2;
int total=0;
void maintain(int p1_size,int p2_size){
    while(p1.size()<p1_size){
        p1.push(p2.top());
        p2.pop();
    }
}
double pq_push(int num){
    total++;
    int p1_size=total/2; //p1的長度會<=p2
    int p2_size=total-p1_size;
    p2.push(num);
    maintain(p1_size,p2_size);
    if(total%2) return p2.top();
    else return (p1.top()+p2.top())/2.0;
}
signed main(){
    cin>>n;
    int tmp;
    for(int i=0;i<n;i++){
        cin>>tmp;
        cout<<double(pq_push(tmp))<<" ";
    }
}
```
## 篩法
```cpp
const int maxn = 1e6 + 5;
bitset<maxn> p;
void seive () {
    p.set();
    p[0] = p[1] = false;
    for (int i = 2; i < maxn; i++) {
        if (p[i]) prime.pb(i);
        for (int j = 0; prime[j] * i < maxn; j++) {
            p[prime[j] * i] = false;
            if (i % prime[j] == 0) break;
        } 
    }
}
```


## dijkstra
```cpp
vector<int> dijkstra (int start, int G) {
    vector<int> dis(n + 1, INF);
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    pq.push({0, start});
    while (pq.size()) {
        auto [x, u] = pq.top();
        pq.pop();
        if (dis[u] != INF) continue;
        dis[u] = x;
        for (auto [v, w] : G[u]) {
            pq.push({w + dis[u], v});
        }
    }
    return dis;
}
```
## spfa
- 如果上一輪某一個點的距離沒有更新,那這一輪也沒必要 relax 他
- 把距離有更新的節點丟進 queue 裡,然後一直拿 queue 裡的節點出來 relax
```cpp
void SPFA (int start) {
    vector<int> dis(n + 1, INF);
    vector<int> inq(n + 1, INF);
    vector<int> cnt(n + 1);
    queue<int> q;
    while (q.size()) {
        int u = q.front();
        q.pop();
        cnt[u]++;
        if (cnt[u] == n) {
            // negative cycle
        }
        inq[u] = false;
        for (auto [v, w] : G[u]) {
            dis[u] = dis[v] + w;
            if (!inq[v]) {
                inq[v] = true;
                q.push(v);
            }
        }
    }
}
```

## 矩陣快速冪
```cpp
Matrix operator* (const Matrix &A, const Matrix &B) {
    Matrix C(n, vector<int>(n));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % M;
            }
        }
    }
    return C;
}

Matrix pow(Matrix A, int k) {
    // 不能 &A 注意!!!(會改到 A)
    int n = A.size();
    Matrix ret(n, vector<int>(n));
    for (int i = 0; i < n; i++) ret[i][i] = 1;
    // 初始化為單位矩陣
    while (k > 0) {
        if (k & 1) ret = ret * A;
        A = A * A;
        k >>= 1;
    }
    return ret;
}
```

## 皇后

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

const int INF = (1LL << 60);
const int maxn = 30;
const int M = 1e9 + 7;

int n, m;
int c[100], qa[100], qb[100], ca[100], cb[100], ans;

void dfs (int Q, int C, int i) {
    if (i == n + m) {
        ans++;
        return;
    }
    if (C) {
        for (int j = 0; j < n + m; j++) {
            if (c[j]) continue;
            if (qa[i - j + maxn] || qb[i + j]) continue;
            c[j] = 1;
            ca[i - j + maxn]++, cb[i + j]++; 
            // 注意 29 行不能 =1 會 bug
            // 因為一個斜向可以有多個城堡
            dfs (Q, C - 1, i + 1);
            c[j] = 0;
            ca[i - j + maxn]--, cb[i + j]--;
            // 注意 34 行不能 =0 會 bug
        }
    }
    if (Q) {
        for (int j = 0; j < n + m; j++) {
            if (c[j]) continue;
            if (qa[i - j + maxn] || qb[i + j]) continue;
            if (ca[i - j + maxn] || cb[i + j]) continue;
            c[j] = qa[i - j + maxn] = qb[i + j] = 1;
            dfs (Q - 1, C, i + 1);
            c[j] = qa[i - j + maxn] = qb[i + j] = 0;
        }
    }
}

void init () {
    cin >> n >> m;
}

void solve () {
    dfs (n, m, 0);
    cout << ans << "\n";
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

## Graph Coloring

### k = 2

- 二分圖

### k = 3

- $O(n \times 2^n)$ 列舉第一種獨立集
- 剩下的二分圖

### k =4

- $dp(S)=S$  是否可以 $\texttt{2-colors}$ 上色 $O(m \times 2^n)$
- 列舉 $S$ 使得 $\texttt{dp(S)}$ 跟 $\texttt{dp(V/S)}$ 都是 $\texttt{true}$

### others

- $dp(S)=S$ 最少多少 coloring
- $dp(S) = \min \limits_{T \in S} \begin{cases} dp(S/T)+1 \end{cases}$ 且 $T$ 是獨立集
- $O(3 ^n)$
