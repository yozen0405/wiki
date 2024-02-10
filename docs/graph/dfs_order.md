## 利用 dfs 序

???+note "[CSES - Network Renovation](https://cses.fi/problemset/task/1704)"
	給一顆 $n$ 個點的樹，問最少加幾條邊可使圖上沒有邊是 bridge (雙連通)
	
	$n\le 10^5$
	
	??? note "思路"
	    
	    > 參考 : <https://www.youtube.com/watch?v=tRTezLvPZ3k>
	    
	    觀察到要連接的是 leaf 跟 leaf
		
		若不是的話往 leaf 的方向那段會斷掉
		
		<figure markdown>
	      ![Image title](./images/24.png){ width="300" }
	    </figure>
	    
	    所以我們**至少**需要 $P_T/2$ 個才能將圖給覆蓋，其中 $P_T$ 為 leaf 的數量
	    
	    因為如果 hooked 的地方不是 leaf，那下面的 edge 就不會被覆蓋到
	    
	    存在一個 pedant centriod，若將樹 rerooted as the pendant centriod
	    
	    每個 subtree 中的 leaf 的數量將會 <= $P_T /2$，所以我們可以 greedy 的每次配當前最大的兩個 subtree，用 prioiryt_queue 可以做到，這個是 $O(n\log P)$ 的解法。
	    
	    其實可以用 i 跟 i + $P_T/2$ 配就一定可以配到「同一個子樹之外」。那假如我們的 root 定在任意點，因為 euler 序列的順序不會因為 root 的改變而改變，所以依然可以照上面的方法將 i 跟 i + $P_T/2$ 配，這個解法是線性時間。
	
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
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    vector<int> G[maxn];
	    int n;
	    vector<int> leaf;
	
	    void dfs (int u, int par) {
	        if (G[u].size() == 1) leaf.pb(u);
	        for (auto v : G[u]) {
	            if (v == par) continue;
	            dfs (v, u);
	        }
	    }
	
	    void init() {
	        cin >> n;
	        int u, v;
	        for (int i = 0; i < n - 1; i++) {
	            cin >> u >> v;
	            G[u].pb(v);
	            G[v].pb(u);
	        }
	    }
	
	    void solve() {
	        dfs (1, 0);
	        if (leaf.size() & 1) leaf.pb(leaf[0]); // 使最後一個連到第一個
	
	        cout << leaf.size()/2 << "\n";
	
	        for (int i = 0; i < leaf.size()/2; i++) {
	            cout << leaf[i] << " " << leaf[i + leaf.size()/2] << "\n";
	        }
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

???+note "[2021 全國賽模擬賽 pF. 地洞遊戲](https://tioj.ck.tp.edu.tw/pmisc/pre-nhspc-2021-statements/Cave.pdf)"
	給定一棵 $N$ 點的有根樹，邊是由根往底下連的
	
	葉節點都有一條新的有向邊連接 $u\to a_u$，其中 $a_u$ 一定是 $1\to \ldots \to u$ 中的一點
	
	同一個葉節點不能 visit 超過一次
	
	從節點 $1$ 出發，問是否能 visit 所有葉節點，構造任意一組葉節點的 visit 順序
	
	$N \le 3\times 10^5$
	
	<figure markdown>
	  ![Image title](./images/18.png){ width="200" }
	  <figcaption>例如此圖的順序就是 $5\to 4\to 2$</figcaption>
	</figure>
	
	??? note "思路"
		[重要性質] : 若存在至少一組合法解，則必存在一組合法解使的他是某個dfs序
		
		[引理] : 可以發現對於定根 $u$ 來說，最多只有一個子樹，他的所有葉子走完以後無法回到 $u$ 或他的祖先
		
	    使用樹 DP。令 $depth_v$ 為 $v$ 的深度，且令 $dp_u$ 為繞完這棵子樹後可以回到的最淺深度。轉移如下：
	
	    - 如果子樹中有至少兩個 $dp_{son}$ 都大於 $depth_u$，則其中一個繞完以後就回不了另一個了，必定無解。
	    
	    - 如果子樹中恰有一個 $dp_{son}$ 大於 $depth_u$，則這個子樹要放到最後才走，$dp_u = dp_{son}$
	    
	    - 如果子樹中沒有 $dp_{son}$ 大於 $depth_u$，則任意一個子樹都能當最後一個走到的，由於我們要深度最淺的，因此 $dp_u$ 就是所有 $dp_{son}$ 的最小值
	
		構造的話找出就直接去 dfs，在過程中先去 dfs 合法的子樹，再去 dfs 不合法的
		
		合法的子樹內的順序可以任意
		
		```cpp
		void dfs (int u) {
			if (u is leaf) print (u);
			
			int last = -1;
			for (auto v : G[u]) {
				if (v is legal) dfs (v);
				else last = v;
			}
			
			if (last == -1) continue;
			
			dfs (last);
		}
		```

???+note "[CF 1528 C. Trees of Tranquility](https://codeforces.com/problemset/problem/1528/C)"
	給出兩棵 root 為 1 的樹，分別稱為 A 樹和 B 樹，現在透過兩棵樹可以構造出一個無向圖，當且僅當點對 (u, v) 同時滿足下列兩個條件時，可在圖中建邊： 

    - 在 A 樹中，u 是 v 的祖先或 u 是 v 的祖先
    
    - 在 B 樹中，u 不能是 v 的祖先同時 u 不能是 v 的祖先
    
    問構造出的無向圖的 maximum clique 的大小是多少
    
    $2\le n\le 3\times 10^5$
    
    ??? note "思路"
    	注意到在一個 clique 內的點一定是在 A 樹的一條 chain 上，在 B 樹內的「dfs 序」不能有交集。所以我們可以將問題轉換成給一些 interval，問 maximum independent set 大小，我們可以用一個 set 表示當前有選在 maximum clique 內的點在 B 中的 interval，當加入一個 interval [l, r] 時 :
    
    	- 若 [l, r] 沒有 overlap 任何 interval，直接 insert，ans++
    	
    	- 若有一個更大的 interval [tl, tr] 包含 [l, r]，將 [tl, tr] erase，insert [l, r]
    	
    	- 若 [l, r] 包含了一個 interval [tl, tr]，不做任何動作最好
    
    	當 dfs 回溯的時候再將 insert 的 erase 掉，有 erase 的 insert 回去即可



