## 引入

啟發式合併的英文叫 small to large method，樹上啟發式合併的話是 dsu on tree

## Disjoint set

??? info "並查集中節點數為 $n$ 的樹，高度為 $O(\log n)$"
	
	因為合併的時候是啟發式合併，子樹大小比較小的放在比較大的下面。這樣合併出來的樹，每次往上走一層，子樹大小都會變至少兩倍，所以高度最多 $O(\log n)$

因為樹的高度至多 $O(\log n)$，所以每個東西至多會被搬運 $O(\log n)$ 次。

???+note "[洛谷 P3224 [HNOI2012] 永无乡](https://www.luogu.com.cn/problem/P3224)"

    給一個 $n$ 點 $m$ 邊的無向圖，有 $q$ 筆操作 :
    
    - $\text{AddEdge}(u,v):$ 在 $u,v$ 之間加一條邊
    - $\text{Query}(u,k):$ 問 $u$ 所在的連通塊第 $k$ 小的編號是多少
    
    $n,m\le 10^5, q\le 3\times 10^5$
    
    ??? note "思路"
    	在啟發式合併時順便維護 Set 即可

???+note "[Codeforces EDU-DSU Step1-C. Experience](https://codeforces.com/edu/course/2/lesson/7/1/practice/contest/289390/problem/C)"

    有 $n$ 個數字初始皆自己一組，有 $q$ 筆操作如下 :
    
    - $\text{join}(x,y):$ 將 $x$ 與 $y$ 所在的組別合併
    - $\text{add}(x,v):$ 將 $x$ 所在的組別的數字皆 $+k$
    - $\text{get}(x):$ 問 $x$ 的數字是多少
    
    $n,q\le 2\times 10^5$
    
    ??? note "思路"
    	每個集合都維護一個懶標。使用啟發式合併的想法，每次 merge 的時候暴力的將比較小的集合的每一樣都加上該集合的懶標，問 get(x) 的時候，直接輸出懶標 + x 上面的數字即可

## 樹上啟發式合併

對於每個節點 $u$，找出最大 size 的 $v_\max$ 稱作重兒子， 剩下 $v$ 稱作輕兒子，將輕兒子維護的東西一個個併入 $v_\max$

??? info "證明 : 從 $\texttt{root}$ 到任意點的輕邊數量 $\le \log n$"

    對於每個輕邊，$size_v$ 必小於 $size_u/2$（不然就是重兒子了），因此排除 size 特別大的重邊，每次往上走一層，子數大小都會變至少 $2$ 倍，所以高度最多 $O(\log n)$，每個點被跑過的次數也為 $O(\log n)$，所以總複雜度也 $O(n\log n)$。
    
    （補充 : 如果從 $\texttt{root}$ 來看的話，子節點越多也代表深度會越淺，雖然 $\texttt{root}$ 會跑到較多輕兒子，但輕兒子被跑到的次數也相對會較少）

???+note "[CSES Distinct Colors](https://cses.fi/problemset/task/1139/)"
	給出一棵 $n$ 個節點以 $1$ 為根的樹，節點 $u$ 的顏色為 $c_u$
	
	現在對於每個點 $u$ 問 $u$ 的子樹裡一共出現了多少種不同的顏色。
	
	$n\le 2\times 10^5$
	
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
	
	    const int INF = (1LL << 60);
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    int n;
	    int a[maxn];
	    vector<int> G[maxn];
	    set<int> S[maxn];
	    int ans[maxn];
	
	    void dfs (int u, int par) {
	        if (G[u].size() == 1 && u != 1) {
	            ans[u] = 1;
	            S[u].insert (a[u]);
	            return;
	        }
	
	        int mx = -1;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs (v, u);
	            if (mx == -1) mx = v;
	            else if (S[mx].size() < S[v].size()) mx = v;
	        }
	        set<int> &t = S[mx];
	        for (auto v : G[u]) {
	            if (v == par || v == mx) continue;
	            for (auto ele : S[v]) t.insert(ele);
	            S[v].clear();
	        }
	        t.insert(a[u]);
	        ans[u] = t.size();
	        swap(t, S[u]);
	    }
	
	    void init() {
	        cin >> n;
	        for (int i = 1; i <= n; i++) cin >> a[i];
	        int u, v;
	        for (int i = 0; i < n - 1; i ++) {
	            cin >> u >> v;
	            G[u].pb(v);
	            G[v].pb(u);
	        }
	    }
	
	    void solve() {
	        dfs (1, 0);
	        for (int i = 1; i <= n; i++) cout << ans[i] << " ";
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

???+note "[CF 600E Lomsat gelral](https://codeforces.com/problemset/problem/600/E)"
	給一顆大小為 $n$ 的有根樹，每個節點 $i$ 的顏色是 $c_i$
	
	對於每個子樹，求出其出現次數最多的顏色，如果有幾個出現次數一樣的顏色，把他們加起來
	
	$c_i\le n \le 10^5$

???+note "[CF 1709 E. XOR Tree](https://codeforces.com/problemset/problem/1709/E)"
	給一棵 $n$ 個點的樹，每個點有權值 $a_i$，一棵樹是「好的」若且唯若所有 path 滿足將 path 上的 $a_i$ xor 起來都不是 0。一次操作中可將某個點上的權值修改成任意數，問最小需要幾次操作才能使樹是「好的」
	
	$1\le n\le 2\times 10^5,1\le a_i < 2^{30}$
	
	??? note "思路"
		令 f(u, v) 是 u 到 v path 上的 $a_i$ xor 起來的值。f(u, v) = f(root, u) ⨁ f(root, v) ⨁ a[ lca(u, v) ]
	
	    對於一個 x 為根的子樹，我們只要去子樹內看有沒有存在兩個點 (u, v) 滿足 f(root, u) ⨁ f(root, v) ⨁ a[x] 是不是 0，如果是 0 的話我們可將 x 改變成 $2^{30+t}$，這樣兩個點都在 x 子樹內的點就不可能與任何點 xor 是 0 了。
	
	    實作上從 leaf 往 root dfs，採用 set + 啟發式合併維護子樹內所有點的 f(root, u)，去看當前 set 內是否有 f(root, u) ⨁ a[x] 這個值即可，若有那我們將 ans++，且不用將任何 f(root, u) 上傳到上面的 parent，因為在 x 改過後不會與其他權值重複，也就代表不可能與任何點 xor 是 0
	    
	    > 更詳細可參考 : <https://zhuanlan.zhihu.com/p/645890524>
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    const int N = 2e5 + 5;
	    int n, ans;
	    int f[N], a[N];
	    vector<int> G[N];
	    set<int> st[N];
	
	    void dfs(int u, int par) {
	        st[u].insert(f[u]);
	
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            f[v] ^= f[u];
	            dfs(v, u);
	
	            if (st[v].size() > st[u].size()) {
	                swap(st[u], st[v]);
	            }
	        }
	
	        bool ok = true;
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            if (ok == false) continue;
	
	            for (auto val : st[v]) {
	                if (st[u].count(val ^ a[u])) {
	                    ok = false;
	                    st[u].clear();
	                    ans++;
	                    break;
	                }
	            }
	            if (ok) {
	                for (auto val : st[v]) {
	                    st[u].insert(val);
	                }
	            }
	        }
	    }
	
	    signed main() {
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i];
	            f[i] = a[i];
	        }
	        for (int i = 1; i < n; i++) {
	            int u, v;
	            cin >> u >> v;
	            G[u].push_back(v);
	            G[v].push_back(u);
	        }
	
	        dfs(1, 0);
	
	        cout << ans << "\n";
	    }
	    ```

## 啟發式合併

???+note "[2023 YTP p10. BST (Building_Spanning_Tree)](https://yozen0405.github.io/wiki/basic/brute_force/images/YTP2023FinalContest_S2_TW.pdf#page=26)"
	給你 $n$ 點的無向圖，一開始圖上沒有任何邊。給你 $n-1$ 條邊恰好會在圖上構成一個 spanning tree，再給你 $m$ 條邊輸出加邊的 order 使得圖恰好會構成這 $n-1$ 條邊的 spanning tree，且 order 字典序越小越好
	
	$n\le 10^5, m\le 2\times 10^5$
	
	??? note "思路"
	    先將 Tree edge 以都丟到一個以字典序小到到排序的 pq，每次看 pq.top()
	
	    - 若為 Tree edge，則看有哪些非 Tree edge 在這輪會可以會變合法，將他們也加入 pq，再將該 Tree edge push back 到答案
	    - 若非 Tree edge，則直接 push back 到答案
	
		至於要怎麼看非 Tree edge 會不會變合法，可以用 Disjoint set 在維護有碰到該連通塊的非 Tree edge 的 set/vector，在 Disjoint set Merge 的時候使用啟發式合併即可，複雜度 $O((n + m) \log (n + m))$

???+note "[USACO 2020 Open Gold p2. Favorite Colors](http://www.usaco.org/index.php?page=viewproblem2&cpid=1042)"
	給一張 $N$ 點 $M$ 邊有向圖，點編號為 $1\ldots N$，每種顏色也可以用 $1\ldots N$ 中的一個整數表示
	
	若 $u\to \{v_1,v_2,\ldots, v_k \}$ 的話 $\{v_1,v_2,\ldots, v_k \}$ 的顏色都要一樣
	
	求 $i=1\ldots N$ 的顏色分配，使得 distinct color 最大
	
	若有多組解，輸出字典序最小的
	
	$N,M\le 2\times 10^5$
	
	??? note "思路"
		我們先決定好每個點是哪一組 (也就是不管字典序)，最後再從 $1\ldots N$ 依序分配
		
		若 $a\to \{b,c\}$ 代表 $b,c$ 的顏色需相同
		
		假如 $b\to \{v_b \}, c \to \{v_c\}$，$v_b$ 與 $v_c$ 的顏色也要相同
		
		所以我們利用類似 topo sort 的作法，依序將顏色相同的點 merge 成一組
		
		在 merge 兩個集合的過程採用啟發式合併
		
		詳見代碼
		
	??? note "code (by USACO)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	
	    void setIO(string s) {
	        ios_base::sync_with_stdio(0); cin.tie(0); 
	        freopen((s+".in").c_str(),"r",stdin);
	        freopen((s+".out").c_str(),"w",stdout);
	    }
	
	    const int MX = 2e5+5;
	
	    int N,M;
	
	    int par[MX],cnt[MX];
	    vector<int> adj[MX], rpar[MX];
	    queue<int> q; 
	
	    void merge(int a, int b) {
	        a = par[a], b = par[b]; 
	        if (rpar[a].size() < rpar[b].size()) swap(a,b);
	        for (int t: rpar[b]) { par[t] = a; rpar[a].push_back(t); }
	        adj[a].insert(end(adj[a]),begin(adj[b]),end(adj[b])); 
	        adj[b].clear();
	        if (adj[a].size() > 1) q.push(a);
	    }
	
	    int main() { 
	        setIO("fcolor");
	        cin >> N >> M;
	        for (int i = 0; i < M; ++i) {
	            int a,b; cin >> a >> b;
	            adj[a].push_back(b);
	        }
	        for (int i = 1; i <= N; ++i) {
	            par[i] = i; rpar[i].push_back(i);
	            if (adj[i].size() > 1) q.push(i);
	        }
	        while (q.size()) {
	            int x = q.front(); if (adj[x].size() <= 1) { q.pop(); continue; }
	            int a = adj[x].back(); adj[x].pop_back();
	            if (par[a] == par[adj[x].back()]) continue;
	            merge(a,adj[x].back());
	        }
	        int co = 0;
	        for (int i = 1; i <= N; ++i) {
	            if (!cnt[par[i]]) cnt[par[i]] = ++co;
	            cout << cnt[par[i]] << "\n";
	        }
	    }
	    ```

???+note "[APIO 2012 Dispatching](https://tioj.ck.tp.edu.tw/problems/1429)"
	給定一個 $n$ 個點，以 $1$ 為根的樹，每個點有 $c_i, w_i$ 兩個屬性，你需要從某個點 $u$ 子樹內選一些點，滿足選出來的點 $\sum w_i \le W$，最大化「選的數量 $\times c_u$」
	
	$1\le n\le 10^5,1\le c_i,w_i,W\le 10^9$
	
	??? note "思路"
		選 $w_i$ 越小的點越好。每個點維護一個 Min Heap，當 Min Heap 裡面的 $\sum w_i$ 超過 $W$，就 pop 直到 $\le W$，Min Heap 合併時採用啟發式合併。

???+note "[CF 1805 E. There Should Be a Lot of Maximums](https://codeforces.com/contest/1805/problem/E)"
	給一棵 $n$ 個點的樹，定義一棵樹的 MAD 值為「出現兩次以上的點權最大值」。對於每條邊，輸出若刪除該邊，形成的兩棵子樹的 MAD 值取 max。
	
	$2\le n\le 10^5$
	
	??? note "思路"
		開兩個 set 分別維護子樹內的資訊以及子樹外的資訊，使用啟發式合併，複雜度 $O(n\log^2 n)$
		
		> 參考自 : <https://www.luogu.com.cn/blog/KnownError/solution-cf1805e>

???+note "[USACO22DEC Making Friends P](https://www.luogu.com.cn/problem/P8907)"
	給一張 n 點 m 邊的圖，第 i 天 i 點消失，i 點周圍的倆倆互相連邊。問有多少種新的邊產生

	$n, m \le 2 \times 10^5$
	
	??? note "思路"
		最暴力的想法，刪掉一個點直接暴力模擬，該連邊的全都一個個連上，很明顯，這個複雜度是 O(n^3) 的。
		
        考慮優化，我們發現，在暴力連邊時，慢慢的，很多很多邊已經連好了，如果我們大費周章去操作連過的邊，將會浪費大量的時間。觀察到整道題的處理有順序，從小到大，那我們思考能不能找到一個點先**暫時**記錄下以後要連的邊，然後隨著操作的進行，每條邊都慢慢連好了。
        
        那就是目前與 i 相連的點中編號最小的點 j，我們把與 i 相連的點都向它連邊，完全圖就可以一步步連好了。考慮正確性，首先，在與 i 相連的點中，最先處理到的肯定是 j，這時，比 j 小的點都已經去旅遊了，比 j 大的點都還在，也都記錄下來了。
		
		【具體實現】:

        我們使用 set，因為方便合併，求最小點也快，還可以防止有重邊。我們發現只有 i 連向比自己編號大的點才可能被算貢獻，所以 set 只記錄比自己大的點，比自己小的點都會在自己之前被刪掉，沒有貢獻。合併的時候，採用啟發式合併，兩個中小的那個 set 併入大的 set 裡面，複雜度多個 $\log n$，總複雜度 $O(n\log^2 n)$。答案計算只需要累加目前 i 點的邊數減去一開始的，求變化量就好了。
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        typedef long long ll;
        using namespace std;

        const int N = 2e5 + 5;
        int n, m, x, y, org[N], p;
        set<int> G[N];
        ll s;

        void merge(int x, int y) {
            if (G[x].size() < G[y].size())
                swap(G[x], G[y]);
            while (G[y].size()) {
                G[x].insert(*G[y].begin());
                G[y].erase(G[y].begin());
            }
            return;
        }

        int main() {
            cin >> n >> m;
            for (int i = 1; i <= m; ++i) {
                cin >> x >> y;
                if (x > y) {
                    swap(x, y);
                }
                G[x].insert(y);
            }
            for (int i = 1; i <= n; ++i)
                org[i] = G[i].size();
            for (int i = 1; i <= n; ++i) {
                if (!G[i].size())
                    continue;
                while (*G[i].begin() <= i)  // 刪除已刪掉的點
                    G[i].erase(G[i].begin());
                s = s + G[i].size() - org[i];
                if (G[i].size() <= 1)
                    continue;
                p = *G[i].begin();
                G[i].erase(G[i].begin());  // 不要加入自己
                merge(p, i);               
            }
            cout << s << '\n';
            return 0;
        }
    	```