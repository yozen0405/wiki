## TOI 2019 p4

???+note "[TOI 2019 p4. 雲霄飛車 (rollercoaster)](https://sorahisa-rank.github.io/oi-toi/2019/problems.pdf#page=5)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，問至少 swap 相鄰的幾次可使陣列變 bitonic
	
	$n\le 10^5$
	
	??? note "思路"
		我們考慮 1，1 一定只能放在最左或最右，看哪邊比較近就放那邊，然後就可以移除 1，變子問題
		
		實作上我們需要一個 data structure 可以算:
		
		- 左邊有幾個 > i
	
		- 右邊有幾個 > i
	
		可以使用 BIT，複雜度 O(n log n)

## CF Split Sort

???+note "[CF 1863 B. Split Sort](https://codeforces.com/contest/1863/problem/B)"
	給一個 $1\sim n$ 的 permutation，輸出最少操作使 permutation 變 $(1,2,\ldots ,n)$
	
	- 選一個 $x$，把 $\le x$ 的在順序不變下拉到前面，再把 $>x$ 的在順序不變下拉到後面
	
	$n\le 10^5$
	
	??? note "思路"
		一個直覺的想法就是從 1 -> n 看上來，若出現逆序的話答案就要 +1。例如 1, 2, 3 都 ok，到 4 的時候發現往左走了，那我們勢必要花一次操作將 4 弄到 3 的右邊去。
		
		---
		
		> 之前想的思路 :
	
		考慮哪個時候要切 x。在 sorted 的 permutation 中，
	
		先考慮 1，若 1 不是在最前面，那我們有兩種選擇 :
	
	    - 花一次操作使 1 移動到第一格
	    - 讓以後的數字帶我到第一格
	        - 考慮從 x 切，那勢必 pos[1] 要小於 pos[2], ..., pos[x] 才能被帶飛
	        - 顯然當 x 越大的時候越難符合，所以我們其實只要考慮 x 最小，也就是 x = 2 的時候
	        - iff pos[1] < pos[2]
	
	    當我們選擇完後，即可將 1 移除，變成子問題
	    
	    總結下來，我們其實只要看 pos[i]>pos[i+1] 的數量即可
	    
	    > 詳細可參考 : <https://www.youtube.com/watch?v=IhMnux8RfQw>

## CSA Swap Pairing

???+note "[CS Academy - Swap Pairing](https://csacademy.com/contest/archive/task/swap_pairing/statement/)"
	給你一個長度為 $n$ 的陣列 $a_1, \ldots ,a_n$，每種數字恰出現兩次，一次操作可以 swap 相鄰的，問最少操作次數使同種數字都相鄰
	
	$n\le 10^5,n$ 為偶數 $, 0\le a_i \le 10^9$
	
	??? note "思路"
		考慮要使第一個數字與跟他相同的數字要相鄰，將比較後面的那個一路 swap 到第二個位置一定不會比較差，因為若他們兩個都 swap 到中間去，那等等其他人經過他們時還會花更多 cost
		
		所以我們將第一個數字做完後，就可以刪掉他們變子問題。可使用 BIT 計算 cost
	
## 附中模競III pG

- https://codeforces.com/gym/375522

## CSES Sorting Methods

???+note "[CSES - Sorting Methods](https://cses.fi/problemset/task/1162)"

## CSES Permutation Inversions

???+note "[CSES - Permutation Inversions](https://cses.fi/problemset/task/2229/)"

## JOI 2019
???+note "[JOI 2019 p3](https://loj.ac/p/3012)"

    給定一個陣列包含 $\texttt{R,W,G}$ 這三種字元
    
    每次可以 $\texttt{swap}$ 相鄰的，求使得一樣的字元不相鄰的最少交換次數
    
    $n\le 400$
    
    ??? note "思路"
    	
        > subtask: only R, G
        - 觀察到 $\texttt{R}$ 跟 $\texttt{R}$ 不會去交換，$\texttt{G}$ 也不會跟自己交換
        - 代表最終陣列一定長成 
            - $\texttt{RGRGRGRG}$...
            - $\texttt{GRGRGRGR}$...
        - 代表我們只要對於一個 $\texttt{R}$ 有多少個 $\texttt{G}$ 從我左邊變到右邊
        - 逆敘數對
            - 把最終陣列從左到右以 $1\sim n$ 編號
            - 對於原本的陣列計算逆序數對即可
    	
    	---
    	
        > subtask: full
        
        - 從上面我們發現同一個字元之間不會有貢獻(不會交換)
    
        - 考慮經典的 bitmask $\texttt{dp(RGWGRW...)}$
    
        - 但沒有必要存自己跟自己的相對順序(沒有貢獻)
    
        - 所以可以把 bitmask 壓成 $\texttt{dp(R,G,W)}$
            - 代表 dp (R 目前放的數量, G 目前放的數量, W 目前放的數量)
    
        - 轉移 $\texttt{dp(R,G,W)}=\begin{cases} \texttt{dp(R-1,G,W) + cost} \\ \texttt{dp(R,G-1,W) + cost} \\ \texttt{dp(R,G,W-1) + cost} \end{cases}$
    
        - 至於 cost 我們就預處理 $suf_R[j][i],suf_G[j][i],suf_W[j][i]$
            - ex: $suf_R[j][i]=$ 前 $j$ 個 $\texttt{R}$ 有多少個在 index i 後面
        - 假設目前是 $\texttt{R}$ 狀態是 $dp(i,j,k)$ 那
            - 令 $idx = pos_R[i]$ (第 i 個 R 在原來陣列的 index)
            - $cost=suf_G[j][idx]+suf_W[k][idx]$


## USCAO Dance Mooves

???+note "[USACO 2021 January 3. Dance Mooves](http://www.usaco.org/index.php?page=viewproblem2&cpid=1091)" 

	??? note "思路"
		- 令 $s_i=i$ 在經過 $K$ 輪會走過幾個 unique index
	    - $p_i$ 在 $K$ 輪最後會到哪個 index
	    - 顯然每 $K$ 輪一個循環，所以我們先算好 $1...K$ 的
	    - $\sum s_i$ 最多只會是 $2K+N$ 
	        - $N$ 一開始的 index，$2K$ 每次 swap 會 update 兩個新的
	    - 可以建圖，邊為 $(i\rightarrow p_i)$
	        - 會形成多個 cycle 因為 in<sub>i</sub> 和 out<sub>i</sub> 都會是 1
	    - $M=KD+R$
	    - 設 $i$ 在的環長度為 $L$ (唯一)
	    - 若 $D>L$ 那都會走到
	    - 反之則會走到 $s_i,s_{p_i},s_{p_i^2},..,s_{p_i^{D-1}}$
	        - 其中 $p_i^x$ 為 $\underbrace{p[p[p[p...}_{x次}[i]]]]$

https://drive.google.com/file/d/1WB9Hnx3itjsBQO0Qk06e1Iszt7jNzhVF/view
