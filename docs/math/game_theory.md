## 賽局 DP

簡單來說就是暴力跑

???+note "[CSES - Stick Game](https://cses.fi/problemset/task/1729)"
	有 $n$ 個石頭，每次可以拿走 $P=\{p_1,p_2,\ldots,p_k\}$ 個，A, B 輪流取，不能拿就輸。問誰贏
	
	$n\le 10^6,k\le 100,1\le p_i\le n$
	
	??? note "思路"

???+note "[LeetCode 877. Stone Game](https://leetcode.com/problems/stone-game/description/)"

???+note "[Atcoder DP Contest L - Deque](https://atcoder.jp/contests/dp/tasks/dp_l)"
	
???+note "[Atcoder DP Contest K - Stones](https://atcoder.jp/contests/dp/tasks/dp_k)"


## Nim Game

### 證明

- 「先手會贏」的狀態，存在一個走法走到「先手會輸」的狀態

- 「先手會輸」的狀態，不管怎麼走都是走到「先手會贏」的狀態

### 例題

???+note "單堆 Nim Game"
	有 $n$ 個石頭，每次要拿 $1\ldots 5$ 個，A, B 輪流拿，不能拿石頭的人就輸了。問誰贏
	
	??? note "思路"
		
		以不嚴謹的角度看，若先手取了 $x$ 個，那麼後手一定可以取 $y$ 使 $x+y=6$，這麼一來當 n % 6 == 0 時，先手就輸了。那當 n % 6 != 0 時，先手就可以先拿走 $x$ 個使得 n % 6 == 0，這樣又變回上面的 n % 6 == 0 先手必輸的 case 了。
		
	    先手可以贏 iff n % 6 != 0
	    
	    - n % 6 != 0 的狀態存在一個走法走到 n % 6 = 0 的狀態
	
	    - n % 6 = 0 的狀態不管怎麼走都是走到 n % 6 != 0 的狀態

???+note "[CSES - Nim Game I](https://cses.fi/problemset/task/1730)"
	有 $n$ 堆石頭，分別有 $a_1, \ldots , a_n$ 個，Alice, Bob 輪流玩一個 game，輪到自己時可以選其中一堆，拿至少一個石頭，不能拿就輸。問誰贏

	$1\le n \le 2\times 10^5,1\le a_i \le 10^9$
	
	??? note "思路"
		n = 1，先手贏，iff $a_1 > 0$。
	    
	    n = 2，先手贏 iff $a_1 \neq a_2$。
	    
	    - $a_1 \neq a_2$ 的狀態 : 
	    	
	    	存在一個走法走到 $a_1 = a_2$ 的狀態
	    	
	    - $a_1 = a_2$ 的狀態 : 
	    	
	    	不管怎麼走都是走到 $a_1 \neq a_2$ 狀態
	    	
	    general $n$ : 先手會贏 iff $a_1 \oplus a_2 \oplus \ldots a_n \neq 0$
	    	
	    - $a_1 \oplus a_2 \oplus \ldots a_n \neq 0$ 的狀態 : 
	    	
	    	存在一個走法走到 $a_1 \oplus a_2 \oplus \ldots a_n = 0$ 的狀態
	    
	    - $a_1 \oplus a_2 \oplus \ldots a_n = 0$ 的狀態 : 
	    
	    	不管怎麼走都是走到 $a_1 \oplus a_2 \oplus \ldots a_n \neq 0$ 的狀態
	        
	        - [11011, 01000, 00101] xor = 10110 → [01101, 01000, 00101] xor = 00000
	
	        - [01011, 11001, 10101] xor = 00111 → [01011, 11001, 10010] xor = 00000

## Grundy number

幫每個狀態定義一個 Grundy number（又稱 SG 函數） $G(x)$，輸的狀態的 $G(x) = 0$

其他的狀態 $G(x) = \text{mex} \{ G(y) \mid x \space 可以到 \space y\}$[^1]

???+note "單堆 Nim Game - 套用 Grundy number"
	有 $n$ 個石頭，每次要拿 $1\ldots 5$ 個，A, B 輪流拿，不能拿石頭的人就輸了。問誰贏
	
	??? note "思路"
		
		我們可以列出轉移式，$G(n)=\text{mex}\{G(n-1),\ldots ,G(n-5) \}$，我們將表格列出來
		
		$$
        \begin{array}{c|ccccccccccccc}
            n & 0 & 1 & 2 & 3 & 4 & 5 & 6 & 7 & 8 & 9 & 10 & 11 & 12\\
            \hline
            G(n) & 0 & 1 & 2 & 3 & 4 & 5 & 0 & 1 & 2 & 3 & 4 & 5 & 0\\
        \end{array}
        $$
        
        可以發現在這個題目 G(n) = n % 6

## Sprague–Grundy theorem

又稱 SG 定理，有 k 盤 Game（不管完任何 Game 都可以），每次可以選擇一個盤面做操作，直到沒有任何盤面能做操作為止，會有 

$$
G(\{x_1,x_2,\ldots, x_k\})=G(x_1)\oplus G(x_2)\oplus \ldots \oplus G(x_k)
$$

??? info "證明"
    
    【數學歸納法 - 證明】: G({X, Y}) = G(X) $\oplus$ G(Y) 
    
    Base case: G(0, 0) = 0 = G(0) $\oplus$ G(0)
    
    Induction step: 已知 G({X, Y}) = mex{ G({x, y}) }，又能走到的 G({x, y}) 已滿足 G({x, y}) = G(x) $\oplus$ G(y)
    
    > 當 G(x) = t，一定會滿足 x 可以走到 grundy 值是 0 ... (t-1) 的狀態
    
    令 G(X) $\oplus$ G(Y) = t，我們要證明 0 ... (t-1) 的 grundy number 都可以走到，且 t 走不到
    
    - G({x, y}) 無法包含到 t :
    	
    	令 u = G(X), v = G(Y)，u $\oplus$ v = t。X, Y 只能動一個，分兩種 case :
    	
    	- 動 X → x: (非 u) $\oplus$ v != t
    
    	- 動 Y → y: u $\oplus$ (非 v) != t
    	
        所以動了一步之後 G(x) $\oplus$ G(y) 一定不是 t
        
    - G({x, y}) 可以完全包含到 0 ... (t-1) :
        
        令 u = G(X), v = G(Y)，如果想要變成 s，看 s 跟 t 從高位看過去哪一位開始 t[i] = 1, s[i] = 0，去看 u[i], v[i] 哪一個是 1 就是去改動他
        
    有了這個證明後，k 盤 Game 就只是將 x[1], ..., x[k] 兩兩合併，也就是兩兩 xor 即可

???+note "[CSES - Nim Game II](https://cses.fi/problemset/task/1098)"
    有 $n$ 堆石頭，分別有 $a_1, \ldots , a_n$ 個，Alice, Bob 輪流玩一個 game，輪到自己時可以選其中一堆，拿 1...3 個石頭，不能拿就輸。問誰贏

	$1\le n \le 2\times 10^5,1\le a_i \le 10^9$
    
    ??? note "思路"
    	先把每一堆想成一個單獨的 game, 計算 G(x) = x % 4，利用 SG 定理將他們 xor 起來

???+note "例題"
	有 $n$ 堆石頭，分別有 $a_1, \ldots , a_n$ 個，Alice, Bob 輪流玩一個 game，每次可以做其中一個操作 

    - 從某一堆拿走 $1$ 個石頭
    
    - 或平分成相同數量的很多堆
    
    不能拿就輸。問誰贏
    
    ??? note "思路"
    	[12] → [4, 4, 4] → [3, 4, 4] → [3, 2, 2, 4]
    	
        SG(6) 
        = mex{ SG(5), SG(3) $\oplus$ SG(3), SG(2) $\oplus$ SG(2) $\oplus$ SG(2), SG(1) $\oplus$ SG(1) $\oplus$ SG(1) $\oplus$ SG(1) $\oplus$ SG(1) $\oplus$ SG(1)}
        = mex{SG(5), SG(2)}
        
        n = 10^5 ，直接計算 SG(1) … SG(n)（枚舉因數，類似篩法），time: O(n log n)
        
        n = 10^9 幫 SG(i) 找規律

???+note "[YTP 2022 高中程式挑戰營 p11](https://www.tw-ytp.org/wp-content/uploads/2022/12/YTP2022FinalContest_S2_TW.pdf#page=32)" 
    有 n 筆 query。每筆給出 x 堆石頭，兩種操作，無法操作就輸
    
    - 選一堆，移除 1 個
    - 選一堆，拆成 1, 2, 3, …, k 個，前提 n = 1+2+…+k
    
    [2, 5, 10] → [1, 5, 10] → [1, 5, 1, 2, 3, 4]
    
    x <= C = 10^9, n <= 2 * 10^5
    
    ??? note "思路"
    	- SG(x) =
    	
            - mex{ SG(x-1) } if x != k(k+1)/2

            - mex{ SG(x-1), SG(1) $\oplus$ SG(2)$\oplus$ ... $\oplus$ SG(k) } if x = k(k+1)/2

        - x = k(k+1)/2 的狀態不會太多，只有 O( sqrt(C) ) 個

        - 假設 SG( k * (k+1)/2 ) = 2, 如何計算 SG( (k+1) * (k+2)/2 )?
            - if x $\in$ [ k * (k+1)/2 + 1, (k+1) * (k+2)/2 - 1], SG(x) = 0 → SG(x+1) = 1

			- 0, 1 交替，可以從上一個 k * (k + 1) / 2 很快的推出來

        - 實作上把 x = k(k+1)/2 都建表算好，過程中可以用一個只會單調遞增的 pointer 紀錄 k 算到哪裡，每個 a[i] binary search 找到小於 a[i] 的第一個 k * (k+1)/2 的地方，即可推出是 0 還是 1

## Tree

### Min Max Tree

???+note "題目"
	給一個 Tree，每個 leaf 上都有一個數字。從 root 開始，A, B 輪流，每次要往下走一層，A 希望停在小的，B 希望停在大的，問最後會停在哪裡
	
	??? note "思路"
		<figure markdown>
	      ![Image title](./images/1.jpg){ width="300" }
	    </figure>
	    
	    從 leaf 往上做上去

???+note "[TIOJ  1092 . A.跳格子遊戲](https://tioj.ck.tp.edu.tw/problems/1092)"
	給一張 $n$ 點 $m$ 邊的 DAG。A, B 從起點往終點輪流跳，跳到終點的人獲勝，問誰獲勝
	
	$n\le 10^4,m\le 10^5$
	
	??? note "思路"
		我們先將 DAG 展開，變成 Tree 比較好套用 Min Max Tree 的概念。設先手是 $0$，後手是 $1$，那麼先手就要取 min，後手取 max。
	
		<figure markdown>
	      ![Image title](./images/11.png){ width="500" }
	    </figure>
		
		我們重新回到 DAG 上考慮，DAG 上終點就是我們 Tree 上的 Leaf，所以我們可以從終點慢慢推回起點，每個點維護先手，與後手的值（這樣兩個點之間才能轉移，u 的先手從 v 的後手轉移，...），最後的答案就是起點的先手值
	    
	    <figure markdown>
	      ![Image title](./images/12.png){ width="300" }
	    </figure>
	    
	    ---
	    
	    這題也可以用下面的 Game Tree 做，一樣是將 Leaf（終點） 先定 Grundy Number = 0，然後慢慢做回起點去

### Game Tree

???+note "[LOJ #10243. 「一本通 6.7 例 3」移棋子游戏](https://loj.ac/p/10243)"
	給一張 $n$ 點 $m$ 邊的 DAG。給定 $k$ 個棋子，A, B 輪流，每步可以將任意一顆棋子沿一條有向邊移動到另一個點，無法移動者輸掉遊戲，問誰先手贏還輸。
	
	$n\le 2000,m\le 6000,1\le k\le n$
	
	??? note "思路"
		因為每個問題都是獨立的，可以利用 SG 定理將他們的 Grundy Number xor 起來。每個問題我們可以將 DAG 想成 Tree，這樣 Leaf 就會是那些 out degree 是 0 的點。先定這些點的 Grundy Number = 0，然後慢慢做回起點去
		
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
	
	    int n, m, k;
	    vector<int> G[maxn];
	    int vis[maxn], sg[maxn];
	
	    int mex(vector<int>& a) {
	        int n = a.size();
	
	        vector<bool> v(n + 1, false);
	        for (int x : a) {
	            if (x <= n) v[x] = true;
	        }
	
	        for (int i = 0; i <= n; i++) {
	            if (v[i] == false) return i;
	        }
	        return -1;
	    }
	
	    int dfs(int u) {
	        if (vis[u]) return sg[u];
	        if (G[u].size() == 0) {
	            sg[u] = 0;
	            return sg[u];
	        }
	        vis[u] = true;
	
	        vector<int> used;
	        for (auto v : G[u]) {
	            used.pb(dfs(v));
	        }
	        sg[u] = mex(used);
	        return sg[u];
	    }
	
	    signed main() {
	        cin >> n >> m >> k;
	        for (int i = 0; i < m; i++) {
	            int u, v;
	            cin >> u >> v;
	            G[u].pb(v);
	        }
	
	        int res = 0;
	        while(k--) {
	            int x;
	            cin >> x;
	            res ^= dfs(x);
	        }
	        cout << (res == 0 ? "lose" : "win") << '\n';
	    } 
		```

## 題目

???+note "[CSES - Stair Game](https://cses.fi/problemset/task/1099)"
	有 $n$ 個石堆編號 $1,2,\ldots ,n$，每堆一開始有 $a_i$ 個。A, B 輪流，每次可以將任意數量的石頭從 $k$ 移到 $k-1$，其中 $k\neq 1$，不能動就輸。問誰贏
	
	$1\le n\le 2\times 10^5,0\le a_i \le 10^9$
	
	??? note "思路"
		相當於在 even 項玩 Nim Game
	
		結論 : 先手會贏 iff $a_2\oplus a_4\oplus a_6\oplus \ldots \neq 0$
		
		- 「先手會贏」的狀態，存在一個走法走到「先手會輸」的狀態
		
			相當於玩 Nim Game，先手直接動 even 項的到 odd 項
			
		- 「先手會輸」的狀態，不管怎麼走都是走到「先手會贏」的狀態
			
			分兩種 case 討論 :
			
			- 動 odd 到 even
				
				下一輪，先手再把同個數的石頭從 even 動到 odd，又回到先手會輸的狀態
				
			- 動 even 到 odd
	
				相當於玩 Nim Game，不管怎麼走都是走到 $a_2\oplus a_4\oplus a_6\oplus \ldots \neq 0$ 的狀態

???+note "[CSES - Grundy's Game](https://cses.fi/problemset/task/2207)"
	有一堆 $n$ 個石頭，A, B 輪流，每次可以將一堆 Split 成兩堆數量不同的，不能動就輸，問誰贏。共 $t$ 筆測資
	
	$t\le 10^5,n\le 10^6$
	
	??? note "思路"
		觀察到若 $n > 2000$ 時先手必勝，$n\le 2000$ 暴力跑，$>2000$ 直接輸出 first
		
???+note "[CSES - Another Game](https://cses.fi/problemset/task/2208)"
	有 $n$ 堆石頭，分別有 $a_1, \ldots , a_n$ 個，Alice, Bob 輪流玩一個 game，輪到自己時可以選其中好幾堆，每堆拿至少一個石頭，不能拿就輸，問誰贏
	
	$1\le n\le 2\times 10^5,1\le a_i\le 10^9$
	
	??? note "思路"
		打表後可以觀察到，$a_i$ 都是偶數時，先手必輸
		
		【證明】:
		
		- 「$a_i$ 有一些奇數」的狀態，存在一個走法走到「$a_i$ 都是偶數」的狀態

		- 「$a_i$ 都是偶數」的狀態，不管怎麼走都是走到「$a_i$ 有一些奇數」的狀態

???+note "[2016 全國賽 p3. 拈 (Nim)](https://tioj.ck.tp.edu.tw/problems/1940)"
	有一堆 $n$ 個石頭，A, B 輪流，每次可從這堆石頭中取走 $1\ldots \lfloor n/k \rfloor$ 顆石頭，問 Grundy number $G(n)$
	
	$n\le 10^9,k=1$ or $2$
	
	??? note "思路"
		$k=1$ 的 case 一定是 n % (n + 1) = n
		
		$k=2$ 的 case 去觀察會發現 even 項都是 n / 2，odd 項恰好是自己能覆蓋的區間的前一個數，可以去遞迴，複雜度 $O(\log n)$
		
		<figure markdown>
	      ![Image title](./images/13.png){ width="500" }
	    </figure>
		
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

        int f(int n) {
            if (n == 0) return 0;
            if (n == 1) return 1;
            if (n == 2) return 0;
            if (n % 2 == 0) return n / 2;
            else return f(n / 2);
        }

        signed main() {
            int n, k;
            cin >> k >> n;
            if (k == 1) {
                cout << n << '\n';
            } else {
                cout << f(n) << '\n';
            }
        } 
		```

???+note "Wythoff's game [洛谷 P2252 [SHOI2002] 取石子游戏|【模板】威佐夫博弈](https://www.luogu.com.cn/problem/P2252)"
	一開始有兩堆個石頭，每個回合可以選一堆，A, B 輪流，每輪可以做其中一個操作 

    - 從其中一堆取走任意數量的石頭
    
    - 或從兩堆取走相同數量的石頭
    
    不能拿就輸。問誰贏

???+note "<a href="/wiki/math/images/C. 取石子遊戲 (kgame).html" target="_blank">2022 IONC C. 取石子遊戲 (kgame)</a>"
	一開始有 $n$ 顆石頭與一個正整數 $k$，有兩個人會輪流取出一些石頭。

    假設遊戲進行了 $m$ 輪，並且取出的石頭數量的序列為 $a_1, a_2, \ldots, a_m$，那麼必須要滿足以下兩個條件：
    
    - 對於 $i = 1, 2, \ldots, m$，$1 \le a_i \le k-1$。
    
    - 對於 $j = 1, 2, \ldots, m-1$，$a_j + a_{j+1} \le k$。
    
    先將石頭取完的那個人獲勝，若是雙方都無法將石頭取完即視為平手。在已知 $n$、$k$ 的情況下，請問誰有必勝策略？在一筆測資中，你需要處理 $T$ 組輸入。
    
    $1 \le T \le 10^5,1 \le k \le 10^{18},1 \le k \le n$
    
    ??? note "思路"
    	例如說先手取了 $x$ 之後，後手一定可以取 $y$ 使 $x+y=K$，也就代表當 n % k = 0 時先手必輸。那麼無解的 case 呢 ? 當 k = 1 時。
    	
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long
        using namespace std;
    
        signed main() {
            int q;
            cin >> q;
            while(q--) {
                int n, k;
                cin >> n >> k;
                if (k == 1) {
                    cout << "RedLeaf\n";
                } else if (n % k == 0) {
                    cout << "Leaf\n";
                } else {
                    cout << "Red\n";
                }
            }
        } 
    	```

???+note "[CF 1194 D. 1-2-K Game](https://codeforces.com/contest/1194/problem/D)"
	有 $n$ 個石頭，每次拿 $1$ 個, $2$ 個, **或** $k$ 個，A, B 輪流拿，不能拿石頭的人就輸了。問誰贏
	
	$0\le n\le 10^9,3\le k\le 10^9$
	
	??? note "思路"
		打表，觀察，詳細可見 [Yuihuang's 題解](https://yuihuang.com/cf-1194d/)

???+note "[codeforces 603C. Lieges of Legendre](https://codeforces.com/problemset/problem/603/C)"

???+note "[codeforces 305E. Playing with String](https://codeforces.com/problemset/problem/305/E)"

???+note "[2022 npsc 高中組初賽 pF.取蜜柑](https://tioj.ck.tp.edu.tw/problems/2303)"
	
	??? note "思路"
		如果數字>1，那可以都視為2，因為(x>1) x就可以決定要一次拿全部或者拿到剩1個，除了1,1,...1,x 和 1,1,1,1,1 都是1/2 ，1,11..x如果位數是偶數那是1/2,奇數是 (x/2+1)/x

---

## 參考資料

- <https://blog.csdn.net/a_forever_dream/article/details/104813148>

- <https://hackmd.io/@cOBW6f27TLewMLxYuitcdw/BktH-2A6_>


[^1]: mex : 最小不存在的非負整數。更詳細介紹見<a href="/wiki/math/special/mex" target="_blank">此處</a>