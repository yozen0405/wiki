## min-max search

## Nim Game

???+note "[CSES - Nim Game I](https://cses.fi/problemset/task/1730)"
	
## Grundy number

幫每個狀態定義一個 Grundy number $G(x)$，輸的狀態的 G(x) = 0

其他的狀態 G(x) = mex_{x 可以到 y}{ G(y) }

mex(S): 回傳最小沒有出現在集合 S 的非負整數

CSES - Missing Number (小沒有出現在集合 S 的非負整數）

## Sprague–Grundy theorem

如果現在同時玩兩個 Game X, Y 會有 G( {X, Y} ) = G(X) ^ G(Y)
同時玩兩個 Game: 有兩個盤面，每一輪可以選其中一個盤面動一步
盤面 Ｘ：一開始有 9 個石頭，每次要拿 [1~5] 個
盤面 Y：一開始有 18 個石頭，每次要拿 [1~4] 個

G(X) = 9 % 6 = 3
G(Y) = 18 % 5 = 3
G({X, Y}) = G(X) ^ G(Y) = 0 
G(9, 18) = mex{ G(9-i, 18), G(9, 18-j) }
如果現在同時玩 k 個 Game X_1, X_2, .., X_k 會有 G( {X_1, X_2, .., X_k} ) = G(X_1) ^ G(X_2) ^ … ^ G(X_k)
證明
G(x) = t 會滿足 x 可以走到 grundy 值是 0 ~ (t-1) 的狀態
G(X ) ^ G(Y) = t 狀態 (x, y) 無法走到 G(X) ^ G(Y) = t 的狀態
本來的狀態的 G(X) ^ G(Y) = t
動了一步之後 G(X) ^ G(Y) 一定不是 t
G(X) ^ G(Y) = t 狀態 (x, y) 可以走到 G(X) ^ G(Y) = 0 .. (t-1) 的狀態
令 u = G(X), v = G(y)
u ^ v 以下所有數字都要可以達成
如果想要變成 s ，去看 s ^ t 的 high bit 是在 u 或 v 
G(X, Y) = mex{ G(X-i, Y) or G(X, Y-j) }
用數學歸納法 
G(0, 0) = 0 = G(0) ^ G(0)
G(X, Y) = mex{ G(X-i) ^ G(Y) or G(X) ^ G(Y-j) }

???+note "[CSES - Nim Game II](https://cses.fi/problemset/task/1098)"
    每次在某一堆拿 1~3 個石頭
    
    ??? note "思路"
    	先把每一堆想成一個單獨的 game, 計算 G(x) = x % 4，利用 SG 定理求 XOR_{i} {x_i % 4}

???+note "例題"
	一開始有一堆 n 個石頭，每個回合可以選一堆，做以下操作

    - 減少 1 個
    - 或平分成相同數量的很多堆

	??? note "思路"
		[12] → [4, 4, 4] → [3, 4, 4] → [3, 2, 2, 4]
        SG(6) 
        = mex{ SG(5), SG(3)^SG(3), SG(2)^SG(2)^SG(2), SG(1)^SG(1)^SG(1)^SG(1)^SG(1)^SG(1)}
        = mex{SG(5), SG(2)}
        n = 10^5 ，直接計算 SG(1) … SG(n)，time: O(n log n)
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

???+note "[CSES - Stair Game](https://cses.fi/problemset/task/1099)"
	
	??? note "思路"
		n = 3
        SG({a}) = 0
        SG({a, b}) = b
        SG({a, 0, 1}) = mex{ (a, 1, 0) } = 0
        SG({_, 1, 1}) = mex{ (a, 0, 1)=0, (a, 2, 0)=2 } = 1
        SG({_, 2, 1}) = mex{ 0, 1, (a, 3, 0)=3 } = 2
        SG({_, b, 1}) = b
        SG({a, 0, 2}) = mex{ (a,1,1)=1, (a,2,0)=2 } = 0
        SG({a, 1, 2}) = mex{ 0, (a,2,1)=2, (a,3,0)=1 } = 1
        SG({a, 2, 2}) = mex{ 0, 1, (a,3,1)=3, (a,4,0)=4 } = 2
        SG({a, b, 2}) = b
        SG({a, 0, 3}) = mex{ (a,1,2)=1, (a,2,1)=2, (a, 3, 0)=3 } = 0
        SG({a, b, 3}) = mex{ 0~(b-1) (a,b+1,2), (a,b+2,1), (a, b+3, 0) } = b
        SG({_, b, _} ) = b
        SG({_, 0, _, 1} ) = mex{ (_,0,_,0)=0 
        SG({_, b, _, 1} ) = mex( (_, b-i, _, 1) or (_, b, _, 0)=b }

## 題目

???+note "[2022 npsc 高中組初賽 pF.取蜜柑](https://tioj.ck.tp.edu.tw/problems/2303)"
	

???+note "[2016 全國賽 p3. 拈 (Nim)](https://tioj.ck.tp.edu.tw/problems/1940)"

???+note "[CSES - Grundy's Game](https://cses.fi/problemset/task/2207)"


	
???+note "[CSES - Another Game](https://cses.fi/problemset/task/2208)"

???+note "[codeforces 603C. Lieges of Legendre](https://codeforces.com/problemset/problem/603/C)"

???+note "[codeforces 305E. Playing with String](https://codeforces.com/problemset/problem/305/E)"
	
---

## 參考資料

- <https://blog.csdn.net/a_forever_dream/article/details/104813148>

