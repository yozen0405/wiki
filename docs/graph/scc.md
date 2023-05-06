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
        if (v == par) continue;
        if (t[v] == 0) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] >= t[v]) {
                // is bridge
            }
        }
        else {
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
