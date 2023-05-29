# 序列交換問題
> 給定 $a_1,...,a_n$，每次可做以下操作，題目會給定操作限制
> - $\texttt{swap(a[i],a[j])}$
>
> 問做少操作次數使得陣列符合某個條件
## TOI 2019 p4

## 附中模競III pG
- https://codeforces.com/gym/375522

## CSES 
- https://cses.fi/problemset/task/1162
## CSES inversion probability

## CSES 
- https://cses.fi/problemset/task/2229/

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

