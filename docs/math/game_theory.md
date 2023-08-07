## min-max search

## Nim Game

### 定理

$G(i)=\text{mex} \{$ 可以轉移到的狀態 $\}$，定義是 $G(i)=j$，那麼代表狀態 $i$ 轉移到 $0\le j<G(i)$，也可以想成 $i$ 狀態的餘數是 $G(i)$。

那會有一種問題:  如果能轉移到的是 $G(i)=\text{mex}\{0,1,5\}$ ，那為甚麼不要選 $G(i)=6$ 而是$G(i)=2$ ? 所以要理解一個事情: $\text{mex}\{\}$ 重要的只是是不是 $0$ 的問題。

### 證明

- 「先手會贏」的狀態，存在一個走法走到「先手會輸」的狀態

- 「先手會輸」的狀態，不管怎麼走都是走到「先手會贏」的狀態

### 例題

???+note "單堆 Nim Game"
	有 $n$ 個石頭，每次要拿 $1\ldots 5$ 個，A, B 輪流拿，不能拿石頭的人就輸了。問誰贏
	
	??? note "思路"

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

幫每個狀態定義一個 Grundy number $G(x)$，輸的狀態的 $G(x) = 0$

其他的狀態 $G(x) = \text{mex} \{ G(y) \mid x \space 可以到 \space y\}$[^1]

## Sprague–Grundy theorem

有 k 盤 Game（不管完任何 Game 都可以），每次可以選擇一個盤面做操作，直到沒有任何盤面能做操作為止，會有 

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
    每次在某一堆拿 1~3 個石頭
    
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
    n 堆石頭，兩種操作，無法操作就輸
    
    - 選一堆，移除 1 個
    - 選一堆，拆成 1, 2, 3, …, k 個，前提 n = 1+2+…+k
    
    [2, 5, 10] → [1, 5, 10] → [1, 5, 1, 2, 3, 4]
    
    ??? note "思路"
    	SG(x) = 
        mex{ SG(x-1) } if x != k(k+1)/2
        mex{ SG(x-1), SG(1)^SG(2)^…^SG(k) } if x = k(k+1)/2
        x <= C = 10^9, n <=2 * 10^5
        x = k(k+1)/2 的狀態不會太多，只有 O( sqrt(C) ) 個
        SG(1)^SG(2)^…^SG(k) 可以用 prefix sum 技巧在 O(1) 計算
        假設 SG( k*(k+1)/2 ) = 2, 如何計算 SG( (k+1)*(k+2)/2 )?
        if x \in [ k*(k+1)/2 + 1, (k+1)*(k+2)/2 - 1], SG(x) = 0 → SG(x+1) = 1
        把 x = k(k+1)/2 都建表算好，每個 a[i] binary search 找到小於 a[i] 的第一個重要值


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
		
???+note "[2022 npsc 高中組初賽 pF.取蜜柑](https://tioj.ck.tp.edu.tw/problems/2303)"
	
	??? note "思路"
		如果數字>1，那可以都視為2，因為(x>1) x就可以決定要一次拿全部或者拿到剩1個，除了1,1,...1,x 和 1,1,1,1,1 都是1/2 ，1,11..x如果位數是偶數那是1/2,奇數是 (x/2+1)/x

???+note "[2016 全國賽 p3. 拈 (Nim)](https://tioj.ck.tp.edu.tw/problems/1940)"

???+note "[CSES - Grundy's Game](https://cses.fi/problemset/task/2207)"

???+note "[CSES - Another Game](https://cses.fi/problemset/task/2208)"

???+note "Wythoff's game [洛谷 P2252 [SHOI2002] 取石子游戏|【模板】威佐夫博弈](https://www.luogu.com.cn/problem/P2252)"
	一開始有兩堆個石頭，每個回合可以選一堆，A, B 輪流，每輪可以做其中一個操作 

    - 從其中一堆取走任意數量的石頭

    - 或從兩堆取走相同數量的石頭

	不能拿就輸。問誰贏

???+note "[codeforces 603C. Lieges of Legendre](https://codeforces.com/problemset/problem/603/C)"

???+note "[codeforces 305E. Playing with String](https://codeforces.com/problemset/problem/305/E)"
	
---

## 參考資料

- <https://blog.csdn.net/a_forever_dream/article/details/104813148>

- <https://hackmd.io/@cOBW6f27TLewMLxYuitcdw/BktH-2A6_>


[^1]: mex : 最小不存在的非負整數。更詳細介紹見<a href="/wiki/math/special/mex" target="_blank">此處</a>
