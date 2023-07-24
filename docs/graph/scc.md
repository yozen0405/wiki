## F

???+ note "F"
    給出一棵 $n$ 個點的樹，接下來有 $m$ 次操作：
    
    -   加一條從 $a_i$ 到 $b_i$ 的邊。
    -   詢問兩個點 $u_i$ 和 $v_i$ 之間是否有至少兩條邊不相交的路徑。

詢問可以轉化為：求 $u_i$ 和 $v_i$ 是否在同一個簡單環上。按照雙連通分量縮點的想法，每次我們在 $a_i$ 和 $b_i$ 間加一條邊，就可以把 $a_i$ 到 $b_i$ 樹上路徑的點縮到一起。如果兩條邊 $(a_i,b_i)$ 和 $(a_j,b_j)$ 對應的樹上路徑有交，那麼這兩條邊就會被縮到一起。

換言之，加邊操作可以理解為，將 $a_i$ 到 $b_i$ 樹上路徑的邊覆蓋一次。而詢問就轉化為了：判斷 $u_i$ 到 $v_i$ 路徑上是否存在未被覆蓋的邊。如果不存在，那麼 $u_i$ 和 $v_i$ 就屬於同一個雙連通分量，也就屬於同一個簡單環。

考慮使用並查集維護。給樹定根，設 $f_i$ 表示 $i$ 到根的路徑中第一個未被覆蓋的邊。那麼每次加邊操作，我們就暴力跳並查集。覆蓋了一條邊後，將這條邊對應結點的 $f$ 與父節點合併。這樣，每條邊至多被覆蓋一次，總複雜度 $O(n\log n)$。使用按秩合併的並查集同樣可以做到 $O(n\alpha(n))$。

本題的維護方式類似於 D 的樹上版本。

## G

???+ note "G"
    無向圖 $G$ 有 $n$ 個點，初始時均為孤立點（即沒有邊）。
    
    接下來有 $m$ 次加邊操作，第 $i$ 次操作在 $a_i$ 和 $b_i$ 之間加一條無向邊。
    
    每次操作後，你均需要求出圖中橋的個數。
    
    橋的定義為：對於一條 $G$ 中的邊 $(x,y)$，如果刪掉它會使得連通塊數量增加，則 $(x,y)$ 被稱作橋。
    
    強制在線。

本題考察對並查集性質的理解。考慮用並查集維護連通情況。對於邊雙樹，考慮維護有根樹，設 $p_i$ 表示結點 $i$ 的父親。也就是不帶路徑壓縮的並查集。

如果第 $i$ 次操作 $a_i$ 和 $b_i$ 屬於同一個連通塊，那麼我們就需要將邊雙樹上 $a_i$ 到 $b_i$ 路徑上的點縮起來。這可以用並查集維護。每次縮點，邊雙連通分量的個數減少 $1$，最多減少 $n-1$ 次，因此縮點部分的並查集複雜度是 $O(n\alpha(n))$。

為了縮點，我們要先求出 $a_i$ 和 $b_i$ 在邊雙樹上的 LCA。對此我們可以維護一個標記數組。然後從 $a_i$ 和 $b_i$ 開始輪流沿著祖先一個一個往上跳，並標記沿途經過的點。一但跳到了某個之前就被標記過的點，那麼這個點就是 $a_i$ 和 $b_i$ 的 LCA。這個算法的複雜度與 $a_i$ 到 $b_i$ 的路徑長度是線性相關的，可以接受。

如果 $a_i$ 和 $b_i$ 分屬於兩個不同連通塊，那麼我們將這兩個連通塊合併，並且橋的數量加 $1$。此時我們需要將兩個點所在的邊雙樹連起來，也就是加一條 $a_i$ 到 $b_i$ 的邊。因此我們需要將其中一棵樹重新定根，然後接到另一棵樹上。這裡運用啟發式合併的思想：我們把結點數更小的重新定根。這樣的總複雜度是 $O(n\log n)$ 的。

大家在講的 `dfn[]` 就是我這邊的 `t[]`

## LCA
```cpp linenums="1"
// LCA
const int maxn = 1e6 + 5;
int n;
int dep[maxn];
int p[20][maxn];
vector<int> G[maxn];

int dfs(int u, int par) {
    p[u][0] = par;
    dep[u] = dep[par] + 1;
    for (auto v : G[u]) {
        if (v == par) continue;
        dfs(v, u);
    }
}

void build() {
    for (int j = 1; j < 20; j++) {
        for (int i = 1; i <= n; i++) {
            p[i][j] = p[p[i][j - 1]][j - 1];
            // 注意這邊迴圈的順序不能弄反
        }
    }
}

int find(int a, int b) {
    if (dep[a] < dep[b]) swap(a, b);
    // a > b
    int dif = dep[a] - dep[b];
    for (int i = 0; i < 20; i++) {
        if (dif & (1 << i)) {
            a = p[a][i];
        }
    }
    if (a == b) return a;
    for (int i = 19; i >= 0; i--) {
        if (p[a][i] != p[b][i]) {
            a = p[a][i];
            b = p[b][i];
        }
    }
    return p[a][0];
}
```
## low 函數
```cpp linenums="1"
// low 
int dfs(int u, int par) {
    low[u] = t[u] = stamp++;
    for (auto v : G[u]) {
        if (v == par) continue;
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        }
        else {
            low[u] = min(low[u], t[v]);
        }
    }
}
```
## bridge
```cpp linenums="1"
// bridge
int dfs(int u, int par) {
    low[u] = t[u] = stamp++;
    for (auto v : G[u]) {
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] >= t[v]) {
                // is bridge
            }
        }
        else if (v != par) {
            low[u] = min(low[u], t[v]);
        }
    }
}
```

以上的 code 在重邊的情況會壞掉，如果要能判重邊的話如下

```cpp linenums="1"
// bridge
int dfs(int u, int pre_eid) {
    low[u] = t[u] = stamp++;
    for (auto [v, eid] : G[u]) {
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] >= t[v]) {
                // is bridge
            }
        }
        else if (pre_eid != eid) {
            low[u] = min(low[u], t[v]);
        }
    }
}
```

## AP
```cpp linenums="1"
// AP
int dfs(int u, int par) {
    low[u] = t[u] = stamp++;
    int cnt = 0;
    for (auto v : G[u]) {
        if (v == par) continue;
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (par != 0 && low[v] >= t[u]) {
                // not root is AP
            }
            if (par == 0 && cnt++) {
             	// root is AP   
            }
        }
        else {
            low[u] = min(low[u], t[v]);
        }
    }
}
```
## BCC
- 注意: 以下 code "沒有" 考慮重邊的情況


```cpp linenums="1"
//BCC
const int maxn = 1e6 + 5;
int n;
int dep[maxn];
int t[maxn];
int low[maxn];
int bcc[maxn];
int stamp = 1, bccID;
stack<int> stk;
vector<int> G[maxn];
int dfs(int u, int par) {
    low[u] = t[u] = stamp++;
    stk.push(u);
    for (auto v : G[u]) {
        if (v == par) continue;
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
        }
        else {
            low[u] = min(low[u], t[v]);
        }
    }
    if (low[u] == t[u]) {
        int tmp;
        bccID++;
        do{
            tmp = stk.top();
            bcc[tmp] = bccID;
            stk.pop();
        } while (tmp != u);
    }
}
```
## SCC
### kosaraju
```cpp linenums="1"
//SCC
const int maxn = 1e6 + 5;
int n;
stack<int> stk;
vector<int> G[maxn];
vector<int> R[maxn];
int vis[maxn];
int scc[maxn];
int sccID;

int dfs1(int u, int par) {
    vis[u] = true;
    for (auto v : G[u]) {
        if (v == par) continue;
        dfs1(v, u);
    }
    stk.push(u);
}

int dfs2(int u, int par) {
    vis[u] = true;
    scc[u] = sccID;
    for (auto v : R[u]) {
        if (v == par) continue;
        dfs2(v, u);
    }
}

int solve() {
    memset(vis, 0, sizeof(vis));
    for (int i = 1; i <= n; i++) {
        if (!vis[i]) dfs1(i, 0);
    }
    memset(vis, 0, sizeof(vis));
    while(stk.size()) {
        if (!vis[stk.top()]) {
            sccID++;
            dfs2(stk.top(), 0);
        }
        stk.pop();
    }
}
```

### targain

```cpp linenums="1"
void dfs (int u) {
    low[u] = t[u] = ++stamp;
    instk[u] = true;
    stk.push(u);
    for (auto v : G[u]) {
        if (t[v] == 0) {
            dfs(v);
            low[u] = min (low[u], low[v]);
        }
        else if (instk[v]) { // 注意
            low[u] = min (low[u], t[v]);
        }
    }
    if (low[u] == t[u]) {
        int x;
        sccID++;
        do {
            x = stk.top();
            stk.pop();
            scc[x] = sccID;
            instk[x] = false;
        } while (x != u);
    }
}
```

### scc縮點
```cpp linenums="1"
#include <bits/stdc++.h>
#define pii pair<int, int>
#define mk make_pair
#define pb push_back
using namespace std;

const int maxn = 1e4 + 5;
vector<int> G[maxn];
vector<int> R[maxn];
vector<int> W[maxn];
int n, m;
int sccID;
int ans = 0;
int vis[maxn];
int sc[maxn];
int in[maxn];
int dp[maxn];
vector<int> scc[maxn];
stack<int> stk;

void dfs1(int u = 1) {
    vis[u] = true;
    for (auto v : G[u]) {
        if(!vis[v]) dfs1(v);
    }
    stk.push(u);
}

void dfs2(int u = 1) {
    vis[u] = true;
    scc[sccID].push_back(u);
    sc[u] = sccID;
    for (auto v : R[u]) {
        if (!vis[v]) dfs2(v);
    }
}

void topo() {
    queue<int> q;
    for (int i = 1; i <= sccID; i++) {
        if(!in[i]){
            q.push(i);
            dp[i] = scc[i].size();
            ans = max(ans, dp[i]);
        } 
    }
    while(q.size()) {
        int u = q.front();
        q.pop();
        for (auto v : W[u]) {
            int sz = scc[v].size();
            dp[v] = max(dp[v], dp[u] + sz);
            ans = max(ans, dp[v]);
            in[v]--;
            if(!in[v]) q.push(v);
        }
    }
}

void init() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        G[i].clear();
        R[i].clear();
        W[i].clear();
        scc[i].clear();
    }
    for (int i = 0, u, v; i < m; i++) {
        cin >> u >> v;
        G[u].pb(v);
        R[v].pb(u);
    }
    sccID = 0;
    ans = 0;
    memset(in, 0, sizeof(in));
    memset(dp, 0, sizeof(dp));
    memset(sc, 0, sizeof(sc));
    memset(vis, 0, sizeof(vis));
    for (int i = 1; i <= n; i++) {
        if(!vis[i]) {
            dfs1(i);
        }
    }
    memset(vis, 0, sizeof(vis));
    while(stk.size()) {
        if(!vis[stk.top()]) {
            sccID++;
            dfs2(stk.top());
        }
        stk.pop();
    }
}

void solve() {
    for (int i = 1; i <= sccID; i++) {
        for (int u : scc[i]) {
            for (int v : G[u]) {
                // 重新建圖
                if (sc[u] != sc[v]) {
                    W[sc[u]].pb(sc[v]);
                    in[sc[v]]++;
                }
            }
        }
    }
    topo();
    cout << ans << "\n"; 
}

signed main() {
    int t;
    cin >> t;
    while(t--) {
        init();
        solve();
    }
}
```

```cpp linenums="1"
#include <bits/stdc++.h>
#define int long long
#define pb push_back
#define mk make_pair
#define F first
#define S second
using namespace std;
 
const int INF = 2e18;
const int maxn = 1e5 + 5;
int n, m;
int low[maxn], t[maxn], instk[maxn], stamp, sccID, scc[maxn], in[maxn], cost[maxn], dp[maxn], w[maxn];
vector<int> G[maxn], W[maxn];
vector<int> sc[maxn];
stack<int> stk;
 
void dfs (int u) {
    low[u] = t[u] = ++stamp;
    instk[u] = true;
    stk.push(u);
    for (auto v : G[u]) {
        if (t[v] == 0) {
            dfs(v);
            low[u] = min (low[u], low[v]);
        }
        else if (instk[v]) {
            low[u] = min (low[u], t[v]);
        }
    }
    if (low[u] == t[u]) {
        int x;
        sccID++;
        do {
            x = stk.top();
            stk.pop();
            scc[x] = sccID;
            instk[x] = false;
            sc[sccID].pb(x);
            w[sccID] += cost[x];
        } while (x != u);
    }
}
 
void init () {
    cin >> n >> m;
    int u, v;
    for (int i = 1; i <= n; i++) cin >> cost[i];
    for (int i = 0; i < m; i++) {
        cin >> u >> v;
        G[u].pb(v);
    }
}
 
void topo () {
    queue<int> q;
    for (int i = 1; i <= sccID; i++) {
        if (in[i] == 0) q.push(i), dp[i] = w[i];
    }
 
    int res = 0;
    while (q.size()) {
        int u = q.front();
        q.pop();
        for (auto v : W[u]) {
            dp[v] = max (dp[v], dp[u] + w[v]);
            res = max (res, dp[v]);
            in[v]--;
            if (in[v] == 0) {
                q.push(v);
            }
        }
    }
    cout << res << "\n";
}
 
void solve () {
    for (int i = 1; i <= n; i++) {
        if (t[i] == 0) {
            dfs (i);
        }
    }
    for (int i = 1; i <= sccID; i++) {
        for (auto u : sc[i]) {
            for (auto v : G[u]) {
                if (scc[u] != scc[v]) {
                    W[scc[u]].pb(scc[v]);
                    in[scc[v]]++;
                }
            }
        }
    }
    topo();
}
 
signed main () {
    // ios::sync_with_stdio(0);
    // cin.tie(0);
    int t = 1;
    //cin >> t;
    while (t--) {
        init ();
        solve ();
    }
}
 
 
/*
10 10
1 1 1 1 1 1 1 1 1 1
2 7
 
*/
```

## 2-SAT
```cpp linenums="1"
struct TwoSAT{
    static const int MAXv = 2*MAXN;
    vector<int> GO[MAXv],BK[MAXv],stk;
    int vis[MAXv];
    int SC[MAXv];
    void imply(int u,int v){ // u imply v
        GO[u].push_back(v);
        BK[v].push_back(u);
    }
    void dfs(int u,vector<int>*G,int sc){
        vis[u]=1, SC[u]=sc;
        for (int v:G[u])
            if (!vis[v])
                dfs(v,G,sc);
        if (G==GO)stk.push_back(u);
    }
    void scc(int n){
        memset(vis,0,sizeof(vis));
        for (int i=0; i<n; i++)if (!vis[i])
            dfs(i,GO,-1);
        memset(vis,0,sizeof(vis));
        int sc=0;
        while (!stk.empty()){
            if (!vis[stk.back()])
                dfs(stk.back(),BK,sc++);
            stk.pop_back();
        }
    }
};

signed main(){
    TwoSAT SAT;
    SAT.scc(2 * n);
    // todo
    for(int i=0;i<n;i++){
        if(SAT.SC[2*i]==SAT.SC[2*i+1]) flg=1;
        // 2*i (+), 2*i + 1 (-)
    }
    if(flg) cout<<"BAD\n";
    else cout<<"GOOD\n";
}

```
### 印出一組解
- 選 topo sort 反向
- $x \rightarrow \neg x$ 
    - 如果我選 $x=\texttt{true}$ 結果會推倒到 $x=\texttt{false}$
    - 但如果我選 $x=\texttt{false}$ 那不會發生任何事情
    - 選箭頭後面的為正確的解



```cpp linenums="1"
#include <iostream>
#include <vector>
#include <stack>
#include <queue>
using namespace std;
  
int n, m, a, b, dfn[200005], stk[200005], low[200005], pa[200005], opp[200005], in[200005], pick[200005], scc, idx;
char c[2];
vector <int> v[200005];
vector <int> v2[200005];
stack <int> st;
  
void tarjan(int x){
    idx++;
    dfn[x] = low[x] = idx;
    st.push(x);
    stk[x] = 1;
    for (auto i:v[x]){
        if (!dfn[i]){
            tarjan(i);
            low[x] = min(low[x], low[i]);
        }
        else if (stk[i]){
            low[x] = min(low[x], dfn[i]);
        }
    }
    if (dfn[x] == low[x]){
        scc++;
        pa[x] = scc;
        int nxt = -1;
        while (nxt != x){
            nxt = st.top();
            st.pop();
            pa[nxt] = scc;
            stk[nxt] = 0;
        }
    }
}
int tr(int x){ // 正變負, 負變正
    if (x <= m) return x+m;
    else return x-m;
}
bool check(){
    for (int i = 1; i <= m; i++){
        if (pa[i] == pa[i+m]) return 0; // pa[i] 紀錄 i 的 scc
        else{
            opp[pa[i]] = pa[i+m];
            opp[pa[i+m]] = pa[i]; // opp 紀錄 x 跟 ~x 對方各自的 scc
            /*
            如果 opp[scc] 有被改到的話, 那就代表說
            前一個改的數(x)跟後一個改的數(y) 他們是互相連通的 (例如 ~x -> y 之類的)
            那麼因為如果有 (~x -> y) 的邊那也就代表有 (~y -> x) 的邊 
            (也不一定是邊但這兩個會互相成立，因為在加入2SAT的時候都是兩個成對的邊一起加入)
            所以一旦 opp[scc] 一被改過後就是那個答案了, 有其他人想再改他的話也只是做 opp[scc] = opp[scc]
            */
        }
    }
    return 1;
}
void build(){
    for (int i = 1; i <= m * 2; i++){
        for (int j:v[i]){
            if (pa[i] != pa[j]){
                v2[pa[j]].push_back(pa[i]); // 加入的邊是topo排序的"反向"
                in[pa[i]]++;
            }
        }
    }
}
void topo(){
    queue <int> q;
    for (int i = 1; i <= scc; i++){
        if (in[i] == 0) q.push(i);
    }
    while (!q.empty()){
        int now = q.front();
        q.pop();
        if (!pick[now]){
            pick[now] = 1;
            pick[opp[now]] = 2;
        }
        for (auto i:v2[now]){
            in[i]--;
            if (!in[i]) q.push(i);
        }
    }
    for (int i = 1; i <= m; i++){
        if (pick[pa[i]] == 1) cout << "+ ";
        else cout << "- ";
    }
}
  
int main() {
    cin >> n >> m; // n 個式子, m 個變數
    for (int i = 0; i < n; i++){
        cin >> c[0] >> a >> c[1] >> b;
        /*
        x + m (-)
        x (+)
        */
        if (c[0] == '-') a += m;
        if (c[1] == '-') b += m;
        v[tr(a)].push_back(b);
        v[tr(b)].push_back(a);
    }
    for (int i = 1; i <= m*2; i++){
        if (!dfn[i]) tarjan(i);
    }
    if (check()){
        build();
        topo();
    }
    else cout << "IMPOSSIBLE\n";
}
```

---

## 資料

- <https://cp-algorithms.com/graph/bridge-searching-online.html>

- <https://oi-wiki.org/topic/dsu-app>
