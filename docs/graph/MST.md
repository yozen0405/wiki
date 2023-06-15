- <https://drive.google.com/file/d/1a1mgK8KFJWNoXATHwi3E6ceStn22QmZl/view>

- IOIC 習題

## 算法概要

??? question "對於每個 node 周圍最小 edge 一定會再 MST 內"
	如果這個點旁邊最小邊e1沒選

    把最小邊加到MST上面，會產生一個環
    
    把這個環上面最大的邊 e2 移除，可以得到一個新的生成樹
    
    新的生成樹的權重比剛剛的MST小，這樣產生矛盾

### Kruskal

每次選 **整張圖** 最小權的邊

??? note "code"
	```cpp linenums="1"
	bool cmp(Edge a, Edge b) {
        return a.w < b.w;
    }
    
    long long Kruskal() {
        sort(edges.begin(), edges.end(), cmp);
        long long ans = 0;

        for (int i = 0; i < m; i++) {
            Edge e = edges[i];

            if (find(e.u) != find(e.v)) {
                merge(e.u, e.v);
                ans += e.w;
            }
        }

        return ans;
    }
    ```

### Prim

去選從**起點**擴張出來的集合**周圍的**最小邊

??? note "code"
	```cpp linenums="1"
	void Prim (int start) {
        vector<int> dis(n, INF);
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, start});
        
        while (pq.size()) {
            auto [d, u] = pq.top();
            pq.pop();
            
            if (dis[u] != INF) continue;
            dis[u] = d;
            
            for (auto [v, w] : G[u]) {
                pq.push({w, v}); // 這行跟 dijkstra 不同
            }
        }
    }
    ```

### Boruvka

對於**目前選到**的每個集合，選他**周圍的**最小邊

對於這種邊數很多，但是很多邊用不到的東西，Boruvka 比較容易快速省略掉不需要的邊（只找需要的出來）

???+note "[0x01 2020 花中一模 pE](https://codeforces.com/group/GG44hyrVLY/contest/297533/problem/E)"
	給 $n$ 個點，點有權值 $a_i$，$i,j$ 加邊的花費為 $a_i+a_j$
	
	另外給 $m$ 條特殊道路 : 表示對 $u,v$ 加邊，花費**只能**為 $w$，而非 $a_u+a_v$
	
	要求將這些點連起來形成樹的最小花費代價
	
	$n,m\le 2\times 10^5,a_i\le 10^{12}$
	
	??? note "思路"
	
	    Boruvka
	    edge_w = only w
	    
	??? note "code"
		```cpp linenums="1"
		#include <algorithm>
	    #include <array>
	    #include <iostream>
	    #include <set>
	    #include <utility>
	    #include <vector>
	
	    using namespace std;
	
	    using Edge = array<int, 3>;
	    const int INF = 1e9 + 7;
	
	    struct DSU {
	        int cc;
	        vector<int> par, sz;
	        vector<set<int>> S;
	
	        DSU(int n = 0) : cc(n), par(n), sz(n, 1), S(n) {
	            for (int i = 0; i < n; i++) {
	                par[i] = i;
	            }
	        }
	        int find(int x) {
	            if (par[x] == x) return x;
	            return par[x] = find(par[x]);
	        }
	        bool join(int u, int v) {
	            u = find(u), v = find(v);
	            if (u == v) return false;
	            if (sz[u] < sz[v]) swap(u, v);
	            par[v] = u;
	            sz[u] += sz[v];
	            for (int x : S[v]) {
	                S[u].insert(x);
	            }
	            S[v].clear();
	            cc--;
	            return true;
	        }
	    };
	
	    int n, m;
	    vector<int> a, rk;
	    vector<vector<int>> G;
	    vector<Edge> edges;
	    DSU dsu;
	
	    void init() {
	        cin >> n >> m;
	        a = rk = vector<int>(n);
	        vector<pair<int, int>> tmp(n);
	        G = vector<vector<int>>(n);
	        dsu = DSU(n);
	        for (int i = 0; i < n; i++) dsu.S[i] = {i};
	        for (int i = 0; i < n; i++) {
	            cin >> a[i];
	            tmp[i] = {a[i], i};
	        }
	        sort(tmp.begin(), tmp.end());
	        for (int i = 0; i < n; i++) {
	            rk[tmp[i].second] = i;
	        }
	        sort(a.begin(), a.end());
	
	        for (int i = 0; i < m; i++) {
	            int u, v, w;
	            cin >> u >> v >> w;
	            u = rk[u - 1], v = rk[v - 1];
	            edges.push_back({w, u, v});
	            G[u].push_back(v);
	            G[v].push_back(u);
	        }
	        for (int i = 0; i < n; i++) {
	            sort(G[i].begin(), G[i].end());
	        }
	    }
	
	    inline bool contain(const vector<int> &vec, int x) {
	        return binary_search(vec.begin(), vec.end(), x);
	    }
	
	    void collect_edges() {
	        bool conti = true;
	        while (conti) {
	            conti = false;
	
	            for (int root = 0; root < n; root++) {
	                if (dsu.find(root) != root) continue;
	
	                const set<int> &S = dsu.S[root];
	
	                vector<int> C;  // 一些不在 S 東西
	                Edge best_edge = {INF, -1, -1};
	
	                int x = *S.begin();
	                for (int i = 0; i < n; i++) {
	                    if (dsu.find(i) == dsu.find(x)) continue;
	
	                    if (contain(G[x], i) == false) {
	                        best_edge = {a[x] + a[i], x, i};
	                        break;
	                    } else {
	                        C.push_back(i);
	                    }
	                }
	                for (int u : S) {
	                    for (int v : C) {
	                        if (contain(G[u], v) == false) {
	                            best_edge = min(best_edge, Edge{a[u] + a[v], u, v});
	                            break;
	                        }
	                    }
	                }
	                if (best_edge[0] != INF) {
	                    conti = true;
	                    int u = best_edge[1];
	                    int v = best_edge[2];
	                    dsu.join(u, v);
	                    edges.push_back(best_edge);
	                }
	            }
	        }
	    }
	
	    void mst() {
	        sort(edges.begin(), edges.end());
	        dsu = DSU(n);
	        long long ans = 0;
	        for (auto e : edges) {
	            int w = e[0];
	            int u = e[1];
	            int v = e[2];
	            if (dsu.find(u) != dsu.find(v)) {
	                dsu.join(u, v);
	                ans += w;
	            }
	        }
	        cout << ans << '\n';
	    }
	
	    int main() {
	        cin.tie(0);
	        cin.sync_with_stdio(0);
	
	        init();
	        collect_edges();
	        mst();
	
	        return 0;
	    }
	    ```

???+note "[CF 1550 F.Jumping Around](https://codeforces.com/problemset/problem/1550/f)"
	給數線上 $n$ 個點 $a_1,\ldots, a_n$，起點為 $a_s$，$q$ 筆詢問 ：

	- 給 $x, k$ ，每步可以從當前的位置跳到值域在 $[d-k, d+k]$ 內的 $a_i$，問能否從 $a_s$ 抵達 $a_x$
	
	$n,q \le 10^6,d,k \le 10^6$

### 習題

???+note "[TIOJ 1795. 咕嚕咕嚕呱啦呱啦](https://tioj.ck.tp.edu.tw/problems/1795)"
	給一張 $n$ 點 $m$ 邊無向圖，邊權非 $0$ 即 $1$，問是否存在一個生成樹邊權總合為 $k$
	
	??? note "思路"
	
		最大 MST & 最小 MST 應用 

???+note "[CF 1468 J. Road Reform](https://codeforces.com/problemset/problem/1468/J)"
	給 $n$ 點 $m$ 邊無向圖，選出一棵生成樹，可以對樹上的邊權值 $+1$ 或 $-1$
	
	使樹上邊權最大值恰為 $k$，求最小操作次數
	
	$n,m\le 2\times 10^5,k\le 10^9$
	
	??? note "思路"
	    kruskal
	    
	    [題解](https://blog.csdn.net/weixin_51797626/article/details/124066368?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-124066368-blog-113801758.topnsimilarv1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-124066368-blog-113801758.topnsimilarv1&utm_relevant_index=2)

???+note "[CF 1095 F. Make It Connected](https://codeforces.com/problemset/problem/1095/F)"
	給定 $n$ 個點，點有權值 $a_i$，$i,j$ 加邊的花費為 $a_i+a_j$
	
	另外給 $m$ 條特殊道路 : 表示對 $u,v$ 加邊，花費也可以為 $w$
	
	要求將這些點連起來形成樹的最小花費代價
	
	$n,m\le 2\times 10^5,a_i\le 10^{12}$
	
	??? note "思路"
	
	    kruskal
	    貪心
	    edge_w = a_x+a_y or w

???+note "[CF 1513 D. GCD and MST](https://codeforces.com/contest/1513/problem/D)"
	有 $n$ 個數，每個數代表一個點，點 $i$ 和點 $i+1$ 之間都有一條權值為 $p$ 的邊，若區間 $[i,j]$ 的最小值等於它們的 $\gcd$，$i$ 和 $j$ 之間連一條區間最小值的邊，求最小生成樹

	$n\le 2\times 10^5,p\le 10^9$
	
	??? note "思路"
		
		https://theriseofdavid.github.io/2020/04/27/Codeforces/Codeforces%201513D/
		
		kruskal 過程想法
	
	??? note "code"
		```cpp linenums="1"
		void solve () {
	        sort (A.begin(), A.end());
	        int sum = 0, cnt = 0;
	        for (int i = 0; i < n; i++) {
	            int x = A[i].first, y = A[i].second;
	            if (x >= m) break;
	            // 往左接, 盡量接, 一定要接起來
	            for (int j = y - 1; j >= 0 && a[j] % x == 0 && !vis[j]; j--) 
	                cnt++, sum+=x, vis[j] = true;
	            // 往右接, 但記得要留一格讓更右邊的集合有辦法來接我們
	            for (int j = y + 1; j < n && a[j] % x == 0 && !vis[j - 1]; j++) 
	                cnt++, sum+=x, vis[j - 1] = true;
	                // 連 j 但因 j 是最外層所以不能 vis[j] = 1 證明如下
	                // 讓他最旁邊那格剛好沒有 vis
	                // vis[u] 定義 u 是否已經固定沒辦法再連邊了 
	                // 若 v 再 u 左邊, 比較右邊的 w (如下圖)
	                // ![](https://i.imgur.com/XAiwOvI.png)
	                // v,u,..,w 
	                // 若 w 想要連邊, 那他找 v 去連一定比 u 困難
	                // v 因為還需要多判斷一個 v 是否 % min == gcd, u 連的話則不用
	                // 所以要連的話一定是找目前集合 [l, r] 最右邊的, 不失一般性
	        }
	        sum += m * (n - 1 - cnt);
	        cout << sum << "\n";
	    }
	
	    void init () {
	        memset(vis, 0, sizeof vis);
	        A.clear();
	        cin >> n >> m;
	        for (int i = 0; i < n; i++) {
	            cin >> a[i];
	            A.pb({a[i], i});
	        }
	    }
	    ```

???+note "[Atcode abc282 E. Choose Two and Eat One](https://atcoder.jp/contests/abc282/tasks/abc282_e) "
	一個盒子中有 $N$ 個球，每個球上寫著一個介於 $1$ 和 $M-1$ 之間的數字。對於 $i=1,2,...,N$，第 $i$ 個球上寫著數字 $A_i$。

    當盒子中剩下兩個或更多球時，重複以下步驟：
    
    - 首先，任意選擇兩個球。
    
    - 然後，得分等於 $(x^y + y^x) \pmod{M}$，其中 $x$ 和 $y$ 是兩個球上的數字
    
    最後，任意選擇其中一個球吃掉，將另一個球放回盒子。
    
    輸出可能獲得的最大總得分。
    
    $N\le 500,M\le 10^9$
    
    ??? note "思路"
        kruskal
        
        將題目轉換成 MST 問題

???+note "[TIOJ 2164. 運送蛋餅](https://tioj.ck.tp.edu.tw/problems/2164)"
	給 $n$ 個三維座標點，$i,j$ 連邊的花費為 $(x_i-x_j)^ 2 + (y_i-y_j)^ 2 + (z_i-z_j)^ 2$，求最小生成樹
	
	$n\le 5000,|x_i|, |y_i|, |z_i| \leq 10^ 5$
	
	??? note "思路"
	
	    prim
	    
	    曼哈頓路徑

???+note "[CF 1245 D. Shichikuji and Power Grid](https://codeforces.com/problemset/problem/1245/D)"
	
	在一個二維平面上面，有 $n$ 個城市，現在每個城市都沒有電
	
	有兩種方案 :
	
	- 建發電站，代價是 $c_i$
	
	- 拉電線，$(|x_i-x_j|+|y_i-y_j|)\times (k_i+k_j)$
	
	現在問你最少花費多少代價，能夠使得全部城市都有電，輸出方案
	
	$n\le 2000,x_i,y_i\le 10^6,c_i,k_i\le 10^9$
	
	??? note "思路"
	
		超級源點

???+note "[CF 125 E. MST Company](https://codeforces.com/problemset/problem/125/E)"
	給 $n$ 點 $m$ 邊連通圖，求最小生成樹滿足與點 $1$ 的度數要恰為 $k$
	
	$n,k\le 5000,m\le 10^5,w_i\le 10^5$
	
	??? note "思路"
	
	    K 度限制生成树
	    
	    Aliens 優化
	
	    <figure markdown>
	      ![Image title](./images/1.png){ width="300" }
	      <figcaption>範例圖</figcaption>
	    </figure>

## 補圖技巧

一般題目都會上限 $m=\text{min}(2\times 10^5, \frac{n\times (n-1)}{2})$, 其中 $m$ 代表 $(u,v)$ 沒有邊的數量

實作上維護一個還沒分到組的 `set`，分到組時把他從 `set` 刪掉

??? note "code"
    ```cpp linenums="1"
    void dfs(int u){
        vis[u] = 1;
        st.erase(u);
        vector<int> ret;
        // v 還沒加進集合的
        for(int v : st) { // 沒出現 -> 有邊
            if(!G[u].count(v)) ret.vush_back(v);
        }
        for(int ele : ret){
            st.erase(ele);
        }
        for(int ele : ret){
            vis[ele]=1;
            dfs(ele);
        }
    }
    ```

???+note "[CF 920 E. Connected Components?](https://codeforces.com/problemset/problem/920/E)"
	給定 $n$ 個點的無向圖，$m$ 條補邊(除了這 $m$條邊，其餘都存在)，求每個連通塊的大小
	
	$n,m\le 2\times 10^5$

???+note "[CF 1242 B. 0-1 MST](https://codeforces.com/problemset/problem/1242/B)"
	給 $n$ 個點的完全圖，邊權只有 $0$ 或 $1$，問最小生成樹
	
	$n,m\le 10^5$

## MST 的唯一性

判斷 MST 是否唯一，如果並非唯一，代表它可以被相同權重的邊給替換，$\Rightarrow$ 對於相同的邊一起去跑

??? note "code"
    ```cpp linenums="1"
    void solve () {
        for (int i = 1; i <= n; i++) par[i] = i;
        sort(E.begin(), E.end(), [](Edge a, Edge b) { return a.w < b.w; });
        int cnt = 0;
        for (int i = 0; i < m;) {
            int r = i;
            while (E[i].w == E[r + 1].w) r++; //[i, r]
            // 判斷有多少合法邊
            for (int j = i; j <= r; j++) {
                int x = find(E[j].u), y = find(E[j].v);
                if (x == y) continue;
                // 是 MST 有可能選到的
                cnt++; 
            }
            // 判斷真正的唯一方案數有幾個
            for (int j = i; j <= r; j++) {
                int x = find(E[j].u), y = find(E[j].v);
                if (x == y) continue; // 已經被併過了, 屬於同一方案
                // 再不同的集合, 唯一的方案
                merge(x, y), cnt--;
            }
            // ans = (合法邊) - (唯一方案數) = 其實加了是重複的方案的邊 
            i = r + 1;
        }
        cout << cnt << "\n";
    }
    ```

???+note "[CF 1108F](https://codeforces.com/contest/1108/problem/F)"
	給 $n$ 點 $m$ 邊無向連通圖，你可以做以下操作 :
    
    - 選一條邊，對其邊權 $+1$，使得圖的最小生成樹唯一
     
    求最小操作次數
    
    $n,m\le 2\times 10^5$

???+note "[CF 160 D. Edges in MST](https://codeforces.com/contest/160/problem/D)"
    給 $n$ 點 $m$ 邊無向帶權連通圖，判斷每個邊的 type

    1. 出現在所有方案中
    2. 完全沒出現在任何方案
    3. 至少出現在一種方案
    
    $n,m\le 10^5$
    
    ??? note "思路"
    	因為對於每條邊都需要看他是哪種性質，所以要判斷特定的邊是否是唯一的就不能只算唯一方案數了，必須用 BCC 去看如下
        ![](https://cdn.discordapp.com/attachments/972879937180692551/1014553797806272583/858cfd42763fb554.png)
    
        [網址](https://blog.csdn.net/m0_56280274/article/details/123765300?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EOPENSEARCH%7ERate-1-123765300-blog-101834844.topnsimilarv1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EOPENSEARCH%7ERate-1-123765300-blog-101834844.topnsimilarv1&utm_relevant_index=2)
    
        他只是想看他是不是"對於跟他邊權一樣"的這些 edge ，唯一的 edge 而已，所以並不需考慮之前邊權比較小的時候留下來的邊，等同於新建一張圖

## 維護環技巧

加入沒選到的，刪掉環上除了他以外最大

實作上使用 LCA, kruskal, dp 配合倍增法建表

以下是一道次小生成樹的模板題

???+note "[LOJ #10133. 「一本通 4.4 例 4」次小生成树](https://loj.ac/p/10133)"
	給一張 $N$ 點 $M$ 邊無向圖，求無向圖的嚴格次小生成樹
	
	$N\le 10^5,M\le 3\times 10^5$

???+note "[CF 609 E. Minimum spanning tree for each edge](https://codeforces.com/contest/609/problem/E)"
    給 $n$ 點 $m$ 邊無向帶權連通圖，對每條邊輸出包含那條邊的最小生成樹
    
    $n, m \le 2\times 10^5$

???+note "[TIOJ 1445](https://tioj.ck.tp.edu.tw/problems/1445)"
	

???+note "[CSES - New Roads Queries](https://cses.fi/problemset/task/2101)"
	給一張 $n$ 個點的圖，依序加入 $m$ 條邊，回答 $q$ 筆詢問 :
	
    - $a,b$ 在加入第幾條邊時連通，或沒有連通
    
    $n,q\le 2\times 10^5$

## 最小差值生成樹

最小化最大最小差

???+note "[CF EDU F. Dense spanning tree](https://codeforces.com/edu/course/2/lesson/7/2/practice/contest/289391/problem/F)"
	給一顆 $n$ 點 $m$ 邊無向圖，第 $i$ 邊的邊權為 $w_i$，求最小差值生成樹
	
	$n\le 10^3,m\le 10^4,w_i\le -10^9\sim10^9$
	
	??? note "code"
	    ```cpp linenums="1"
	    sort (Edges)
	
	    for (int i = 1; i <= m; i++) {
	        // mn 為 E[i].w, mx 用 Kruskal 找
	        Dsu_init(); // O(n)
	        for (int j = i; j <= m; j++) {
	            // Kruskal O(m)
	        }
	        ans = min (ans, mx - mn);
	    }
	    // tot: O(nlgn + m^2)
	    ```

???+note "[宜中校內賽 2022 pE](https://sorahisa-rank.github.io/sh-ylsh/2022/problems.pdf#page=12)"
	給一顆 $n$ 點 $m$ 邊無向圖，第 $i$ 邊的邊權為 $w_i$，求最小差值生成樹
	
	$n\le  2\times 10^5, m\le 2\times 10^5, w_i\le 100$
	
	??? note "code"
	    ```cpp linenums="1"
	    sort (Edges) // {w, u, v}
	
	    for (int i = 1; i <= C; i++) {
	        int idx = Edges.lower_bound({i, 0, 0}) - Edges.begin();
	        Dsu_init(); // O(n)
	        for (int j = idx; j <= m; j++) {
	            // Kruskal O(m)
	        }
	        ans = min (ans, mx - mn);
	    }
	    // tot: O(nlgn + C(n + m))
	    ```

## 最大化最小邊

又稱最小瓶頸生成樹

???+note "[TIOJ 1340. 突變史萊姆 (Slime)](https://tioj.ck.tp.edu.tw/problems/1340)"
    給你 $q$ 個詢問，每筆 $a_i$ 要進行以下操作讓其變成 $b_i$（$1\le a_i,b_i\le n$） :

    - 將 $a_i$ 加或減一個 $a_i$ 的因數，但加完或減完後的數字也要在 $1\sim n$ 之間
    
    輸出最大化過程中加/減的倍數中的最小值
    
    $n, q\le 10^5$

???+note "[Zerojudge j125. 4. 蓋步道](https://zerojudge.tw/ShowProblem?problemid=j125)"
	給一個 $n\times n$ 的 grid，每個點有高度 $h_{i,j}$
	
	求從 $(1,1)$ 走到 $(n, n)$ 的最大高度差最小可以是多少
	
	$n \le 300, h_{i,j} \le 10^6$

### 法1 : Greedy

從小到大枚舉 => Kruskal 最大生成樹

複雜度 $O(m\log m)$

### 法2 : 二分搜

DFS/BFS check 只選邊權 $\le x$ 的是否能連通

複雜度 $O(m\log m)$

### 法 3

每次取 $x=$ 剩餘的 $\text{edge}$ 的中位數，檢查圖有沒有連通

- 如果連通 :  $>x$ 的 $\text{edge}$ 都用不到 (刪掉) $\rightarrow \begin{cases} \text{edge} \space少一半 \\ \text{vertex} \space不變 \end{cases}$
- 如果連通 :   $\le x$ 的連通塊縮點 $\rightarrow \begin{cases} \text{edge} \space少一半 \\ \text{vertex} \space變少 \end{cases}$

那麼時間複雜度 ?

$$
T(n,m)=T(n,\frac{m}{2})+O(n+m)
$$

若 $m < n$ 的話那一定是 $\texttt{IMPOSIIBLE}$

所以時間複雜度只需考慮 $m$ 

$$
\begin{align} T (m) &= T(\frac{m}{2}) + O(m) \\ &= O(m) \end{align}
$$


!!! question "為何是 $O(m)$"
	無窮等比級數 $\displaystyle a + ar + ar^2 + \dots = a\frac{1}{1-r}$ 

	$\displaystyle a=n,r=\frac{1}{2}$ 我們得到 $\displaystyle n + \frac{n}{2} + \frac{n}{4} + \dots = n\frac{1}{1-\frac{1}{2}} = 2n.$

## 最小瓶頸路

最大邊最小化路徑

???+note "[LOJ #136. 最小瓶颈路](https://loj.ac/p/136)"
	給定一個 $n$ 點 $m$ 邊的圖，邊有權值，回答 $k$ 個詢問 : 
	
	- 從 $s$ 到 $t$ 的一條路徑，使得路徑上權值最大的一條邊權值最小
	
	$n\le 1000,m\le 10^5,k\le 1000$

1. Kruskal 建最小生成樹，跑 LCA
2. Prim 變化

<figure markdown>
  ![Image title](./images/34.png){ width="300" }
</figure>


## 乘積生成樹

補 : 真正的乘積生成樹

???+note "[全國賽 2016 第二可靠路網](https://sorahisa-rank.github.io/nhspc-fin/2016/problems.pdf#page=9)"
	給一張 $n$ 點 $m$ 邊圖，每個邊上有邊權 $\displaystyle w=\frac{p}{q}$，有重邊
	

	$$cost=\prod w_i$$
	
	求嚴格次小生成樹的 $cost$，以最簡分數 $\displaystyle \frac{p}{q}$ 的形式輸出
	
	$n\le 3000,m\le 5\times 10^5$
	
	??? note "思路"
		將原本的 Kruskal 用加的改成用乘的
		
		因為我們考慮取 log，假如 log a, log b, log c 是最小的，那 a, b, c 也會是最小的，只不過是用乘的
		
		分數乘法可見此處<a href="/wiki/other/fraction/" target="_blank">此處</a>
		
		然後就套用次小生乘樹模板即可

## GCD MST

???+note "[2023 IOIC day2 NewWorld Online](https://judge.ioicamp.org/contests/2/problems/207)"
	給一張 $n$ 個點的圖，點有權重 $a_i$，兩點連邊的權重為 $\gcd(a_i, a_j)$，問最大 MST 

	$1 \le n \le 10^5, 1 \le a_i \le 10^6$
	
	??? note "思路"
		使用數論篩法技巧，每次將同一個因數的點 merge

???+note "[LeetCode 1579. Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)"
    給一張 $n$ 個點 $m$ 邊無向圖，邊有三種 type ：

    - Alice 的邊

    - Bob 的邊

    - Alice 跟 Bob 的邊

    最多可以移除多少條邊使得分別對於 Bob 跟 Alice 圖都還是連通的

    $n,m \le 10^5$

???+note "[USACO Open 2021 Gold P2.Portals](http://www.usaco.org/index.php?page=viewproblem2&cpid=1138)"

???+note "[Atcoder abc210 E. Ring MST](https://atcoder.jp/contests/abc210/tasks/abc210_e)"
	
    給一張 $n$ 點無向圖，點編號 $0,\ldots ,n - 1$，一開始沒有任何邊，有 $m$ 種操作，每種操作都可做任意次 ：
    
    - 給 $a_i, c_i$,選任意一個點 $x$, 連接 $x + a_i \pmod{n}$ 與 $x$，花費 $c_i$
    
    輸出最少讓圖連通的花費
    
    $n\le 10^9,m\le 10^5$

???+note "[Atcoder arc076 B.Built?](https://atcoder.jp/contests/arc076/tasks/arc076_b)"
	給 $n$ 個二維座標點 $(x_i, y_i)$，兩點要建邊的 $\text{cost} = min(|x_i - x_j|, |y_i - y_j|)$，求最小生成樹

	$n\le 10^5, x_i, y_i \le 10^9$

???+note "[Atcoder abc270 F. Transportation](https://atcoder.jp/contests/abc270/tasks/abc270_f)"
	
	有 $n$ 個島嶼，序列 $X, Y$，和 $m$ 條建邊的方案 $(u, v, w)$，每次可以建立以下三種道路
	
	- 兩個島嶼直接連接一個橋，花費 $w$
	
	- 在某一個島上建立港口，花費 $X_i$，所有建立港口的島嶼之間可以互相抵達
	
	- 在某一個島上建立機場，花費 $Y_i$，所有建立機場的島嶼之間可以互相抵達
	
	求最小花費使得所有島嶼跟島嶼連通
	
	$n,m\le 2\times 10^5,X_i,Y_i,w_i\le 10^9$
	
	??? note "思路"
		<https://atcoder.jp/contests/abc270/submissions/41877092>

???+note "[CF 196 E.Opening Portals](https://codeforces.com/contest/196/problem/E)"
	給一張 $n$ 點 $m$ 邊的連通圖，其中有 $k$ 個點是特殊點。一開始在編號 $1$ 的點，在一個特殊點上時，你可以傳送到任意一個走過的特殊點，問走過所有特殊點的最小的距離總和
	
	$n,m,k\le 10^5$

???+note "[CF 891 C.Envy](https://codeforces.com/contest/891/problem/C)"
	給一張 $n$ 點 $m$ 邊的連通圖，有 $q$ 筆詢問，每次給一個集合，包含 $k_i$ 條圖上的邊，求存不存在一棵最小生成樹包含集合內所有的邊
	
	$n,m,q\le 10^5$
