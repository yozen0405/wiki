## 換根 DP

???+note "[2023 IOIC 403 .樹直徑](http://127.0.0.1:8000/wiki/graph/images/1.html)"
	有一棵 $N$ 個點的樹
	
	進行如下操作**一次**：砍掉樹上的其中一條邊，再另外加上一條邊回去使其保持連通。
	
	可以透過這個操作讓樹直徑最小為何？
	
	??? note "code"
		```cpp linenums="1"
		void diameter_fa(int u, int par) {
	        for (int v : adj[u]) {
	            if (v == par) continue;
	            // 斷邊 (u, v)，u 以上含 u 的連通塊的答案
	
	            // 清除 v 在 ans(u) 的貢獻
	            erase_one(ch_dp[u], dp_ch[v]);
	            erase_one(ch_hei[u], hei[v] + 1);
	
	            // 計算答案 max (樹直徑, 往上或往下的最大高度+往上或往下的次小高度)
	            dp_fa[v] = mav(*rbegin(ch_dp[u]), *rbegin(ch_hei[u]) + *nevt(rbegin(ch_hei[u])));
	
	            ch_dp[v].insert(dp_fa[v]);
	            // 由 v -> u -> ... 往上的最大高度
	            ch_hei[v].insert(*rbegin(ch_hei[u]) + 1);
	
	            // 加回 v 在 ans(u) 的貢獻
	            ch_dp[u].insert(dp_ch[v]);
	            ch_hei[u].insert(hei[v] + 1);
	
	            diameter_fa(v, u);
	        }
	    }
		```

???+note "[CF 1029F Tree with Maximum Cost](https://codeforces.com/contest/1092/problem/F)"
	求 $\max \limits_{v=1... n} \{\space \sum\limits_{i = 1}^{n} dist(i, v) \cdot a_i \space \}$
	
???+note "[Balkan OI 2017 City Attractions ](https://www.acmicpc.net/problem/14875)"
	給一顆樹，有一個人第一天在 $1$ 這個節點
	
	每天他會從昨天停下來的點 $x$ 走到點 $y$
	
	$y$ 滿足 $y\neq x$ 且 $a_y-\text{dis}(x,y)$ 是最小的，若這樣還是有多個 $y$ 就選 index 最小的
	
	問第 $K$ 天會在哪個點停下

## 樹 DP

???+note "[CF 461B Appleman and Tree](https://codeforces.com/problemset/problem/461/B)"
	給出一棵樹，每個點是白色或者黑色，問有多少種方案能夠通過去掉一些邊使每個聯通塊中只有一個黑色的點
	
???+note "[2020 TOI pB.建設人工島](https://tioj.ck.tp.edu.tw/problems/2189)"
	求嚴格次長樹直徑

## 其他類型

???+note "[2021 附中模競 II 惡地之路](https://drive.google.com/file/d/1ISO-o4DrQmbuqVVAgxeVQEO3ifMvcy01/view)"
	給一張圖，令 $s$ 到節點 $i$ 走 $k$ 步的最短距離是 $d(i,k)$，對於每個 $i$ 求 $\min \{ d(i,k) \times k \}$

???+note "[2021 全國賽模擬賽 pF. 地洞遊戲](https://tioj.ck.tp.edu.tw/pmisc/pre-nhspc-2021-statements/Cave.pdf)"
	給定一棵有根樹，這棵樹不計根節點一共有 $K$ 個葉子
	
	每個葉子 $i$ 有一個數字 $a_i$，其中 $i$ 必定是 $a_i$ 的祖先
	
	建一張 $K$ 個節點的新有向圖，其中 $i \to j$ 有連邊若且唯若 $a_i$ 同時是 $i$ 和 $j$ 的祖先
	
	問新圖有沒有一條漢米頓路徑，如果有的話輸出任意一條。

???+note "[LOJ #2780. 「BalticOI 2016 Day1」上司们](https://loj.ac/p/2780)"
	給一顆樹，每個點有一個權重 $a_i$ 
	
	對於 $u=1\sim n$，令 $u$ 的小孩是 $v$，滿足 : $\sum a_v < a_u$
	
	輸出 $\sum\limits_{i=1}^n a_i$ 最小可以是多少
	
???+note "[CSES - Network Renovation](https://cses.fi/problemset/task/1704)"
	給一顆樹，你要加一些邊使不管斷任何一條邊斷掉整張圖還是連通的
	
	問最少斷幾條邊，並且輸出任意一組解
	
	class 17 

???+note "[2023 TOI 模擬賽第三場 pE](http://127.0.0.1:8000/wiki/cp/contest/images/TOI-2023-3.pdf#page=11)"
	待補
	

- <https://usaco.guide/gold/all-roots?lang=cpp>

## 樹上背包

???+note "[CF 440D Berland Federalization](https://codeforces.com/problemset/problem/440/D)"
	給你一棵 $n$ 個點的樹，現在要你刪除盡可能少的邊，使得剩餘一個節點數剛好為 $k$ 的子樹
	
	你需要輸出節點數和刪除的邊的編號
	
	$n,k\le 400$
	
	??? note "思路"
		樹上背包

## 樹上啟發式合併

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
	            for (auto ele : S[v]) t.insert (ele);
	            S[v].clear();
	        }
	        t.insert (a[u]);
	        ans[u] = t.size();
	        swap (t, S[u]);
	    }
	
	    void init () {
	        cin >> n;
	        for (int i = 1; i <= n; i++) cin >> a[i];
	        int u, v;
	        for (int i = 0; i < n - 1; i ++) {
	            cin >> u >> v;
	            G[u].pb(v);
	            G[v].pb(u);
	        }
	    }
	
	    void solve () {
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

???+note "[USACO 2020 Open Gold p2. Favorite Colors](http://www.usaco.org/index.php?page=viewproblem2&cpid=1042)"
	給一張有向圖，點編號為 $1\ldots N$，每種顏色也可以用 $1\ldots N$ 中的一個整數表示
	
	若 $u\to \{v_1,v_2,\ldots, v_k \}$ 的話 $\{v_1,v_2,\ldots, v_k \}$ 的顏色都要一樣
	
	求 $i=1\ldots N$ 的顏色分配，使得 distinct color 最大
	
	若有多組解，輸出字典序最小的
	
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

## 類 topo sort

???+note "[2014 全國賽 p4](https://cs.cysh.cy.edu.tw/competition_problem_set/%E5%85%A8%E5%9C%8B103.pdf#page=7)"
	給一顆邊有權重的樹，選一個連通塊，連通塊內所有的邊權總和不能超過 $L$
	
	問所有「沒被選到的點」到「有被選到的點」的最短距離最大的，最少可以是多少
	
	<figure markdown>
      ![Image title](./images/14.png){ width="300" }
    </figure>

	
	??? note "思路"
		顯然我們沒辦法去直接維護要選那些邊作為連通塊會是 optimal 的
		
		那不如反著做 ?
		
		一開始我們把全部的邊都選起來
		
		慢慢從 leaf 開始移除，每次移除時要選擇對答案影響最小的，直到當前未移除的連通塊的邊權總和 $\le L$
		
		過程利用 pq 維護
		
## DP

???+note "[2014 全國賽 p3](https://cs.cysh.cy.edu.tw/competition_problem_set/%E5%85%A8%E5%9C%8B103.pdf#page=9)"
	給 $n$ 個關卡，跟一個集合點
	
	關卡 $i$ 有一條有向邊連接著關卡 $i+1$，邊權是 $c_i$
	
	集合點與每個關卡 $i$ 之間都有一條雙向邊，邊權是 $d_i$
	
	每個關卡上都有價值 $p_i$，拿過了就會消失
	
	<figure markdown>
      ![Image title](./images/13.png){ width="300" }
    </figure>

	
	給你 $R$ 個士兵，一個士兵只能從集合點出發繞過一些路徑回到集合點，且走過的邊權總和不能超過 $B$，問這些士兵最大可取得的價值總合為多少
	
	??? note "思路"
		我們考慮 dp
		
		令 $dp(i,j)$ 關卡 $1\sim i$，派出 $j$ 個人可獲得的最大價值
		
		$$dp(i,j)=\max\limits_{k=1\sim i} \begin{cases}dp(i-1, j)+\text{cost}(i)\\ dp(k-1,j-1)+\text{cost}(k, i) \texttt{ if ok}(k, i) \end{cases}$$
		
		假設 $\texttt{ok}(k, i)$ 是 $O(1)$，dp 的時間就是 $O(n^2)\times O(n)=O(n^3)$
		
		如何改成 $O(1)$ 呢 ?
		
		可以觀察到 $k$ 越小越好，所以我們對於每個 $i$ 直接挑最小合法的 $k$ 即可
		
		所以我們必須先有 $\texttt{ok}(l, r)$ 來判斷說區段合不合法才能來找 $k$
		
		- $a_i:$ 集合點到關卡 $i$ 的 最短距離
			- $a_i=\min \{d_i,a_{i-1}+c_{i-1} \}$
		
		- $b_i:$ 關卡 $i$ 回到集合點的最短距離
			- $b_i=\min \{d_i, b_{i+1}+c_{i+1} \}$
		
		$\texttt{ok}(l, r):a_l+b_r+ \texttt{sum}(c_l,\ldots, c_r)$
		
		利用 $\texttt{ok}(l,r)$ 在 $O(n^2)$ 對於每個 $i$ 預處理最小合法的 $k$

## Greedy
- TOI 2022 pC
- [BalticOI B1](https://codeforces.com/contest/1387/problem/B1)
- [BalticOI B2](https://codeforces.com/contest/1387/problem/B1)


## Prufer code
- https://oi-wiki.org/graph/prufer/#pr%C3%BCfer-%E5%BA%8F%E5%88%97%E7%9A%84%E6%80%A7%E8%B4%A8
- CSES prufer code
- [nhspc 2022 pG](https://sorahisa-rank.github.io/nhspc-fin/2022/problems.pdf)
## Tree Isomorphism
- CSES
## Euler Tour

- CSES path queries
- subtree queries
- [nhspc 2021 pG](https://tioj.ck.tp.edu.tw/problems/2257)



???+note "[LOJ #3943. 「JOI 2023 Final」训猫](https://loj.ac/p/3943)"
	給你一個 $n$ 個點的樹，每個節點有高度 $h_i$，節點的高度是一個 permutation
	
	有一隻貓現在在 $h_i=n$ 的那個節點上
	
	你可以一直刪點，若刪到貓所在的節點那貓就會移到當前連通塊內高度最高的點
	
	求貓每次移動的距離總和最大可以是多少
	
	??? note "思路"
		假設我們目前的圖長這樣
		
		<figure markdown>
	      ![Image title](./images/10.png){ width="400" }
	      <figcaption>$n=14$，藍色的字為每個節點的 $h_i$</figcaption>
	    </figure>
		
		當我們刪掉 $h_i=14$ 的點後，貓可以選擇往三個連通塊的最高點跑，然後執行子問題
		
		假如 $h_i=14$ 的子問題算出來的答案是這樣
		
		<figure markdown>
	      ![Image title](./images/11.png){ width="400" }
	    </figure>
	    
	    那麼答案就是 
	    
	    $$ans_u=\max \{ ans_x+\text{dis}(u,x) \}$$ 
	    
	    其中 $x$ 是 $v$ 的連通塊內高度最高的節點
	    
		可是若以此分治，考慮樹是一條 chain，那最糟可達 $O(n^2)$
		
		---
		
		我們依照上面子問題執行的步驟，建一個新的樹
		
		<figure markdown>
	      ![Image title](./images/9.png){ width="300" }
	      <figcaption>邊權為兩點間的 distance</figcaption>
	    </figure>
	    
	    那麼整題的答案就是樹高度
	    
	    重點是要怎麼建立這個樹呢 ?
	    
	    我們發現我們每次在做的事情就是一直斷邊，$h_i$ 越小的越晚斷
	    
	    所以我們可以倒著想，我們從 $h_i=1$ 開始，每次 merge 周圍最小的節點
	    
	    再來看看 $h_i=2$ 是否已備 merge 過，若還沒再繼續 merge，以此類推
	    
	    以新圖來看我們在做的事情就是從 leaf 慢慢把樹蓋上去
	    
	    至於要怎麼 merge，我們每個點用 pq(min heap) 維護周圍節點的 $\texttt{pair}(h_i,i)$ 
	    
	    兩個點 merge 起來用啟發式合併，但要注意可能會算到已算過的節點，所以在 pop 的時候檢查此編號是否已經在同一個 DSU 內

