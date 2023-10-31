???+note "[CF 1886 E - I Wanna be the Team Leader](https://codeforces.com/contest/1886/problem/E)"
    有 $m$ 個 projects，難度分別為 $b_1, \ldots ,b_m$，有 $n$ 個人，抗壓程度分別為 $a_1, \ldots ,a_n$。現在需要分組，滿足以下條件 :

    - 使每個 project 至少有一個人做
    - 每個人至多負責一個 project
    - 令負責第 $j$ 個 project 的人有 $k$ 個，則這些 $a_i$ 須滿足 $a_i\le\frac{b_j}{k}$
    
    $n\le 2\times 10^5, m\le 20,1\le a_i, b_i\le 10^9$
    
    ??? note "思路"
        考慮將序列 $a$ 大到小 sort，每個 $b_i$ 挑的就會是一個 prefix，因為如果不是一個 prefix，那對於最後面的那格我們只在意前面選的數量，因此可將倒數第二格與前面的某個的 $b_i$ swap，還可以讓其他 $b_i$ 挑的 $a_i$ 的最小值變大，一直這樣做就會變好幾個連續區段。那問題就可以用 bitmask dp 解決，$dp[S]$ 代表 $S$ 目前 prefix 選到哪裡可使 $S$ 內的 $b_i$ 都滿足條件，轉移的話枚舉不在 $S$ 內的 $j$，看 $j$ 要延伸到哪裡，我們令 $j$ 支配的區段為  $[dp[S]+1,dp[S\cup j]]$，$dp[S\cup j]=nxt(j,dp[S])$。$nxt(j,i)$ 代表 $b_j$ 從 $i$ 開始往右選最少選到哪裡即可使 $b_j$ 滿足條件。



## Graph coloring problem

最小點著色、 k 點著色，都是 NP-complete 問題

??? info "四色定理"
	平面圖一定可以四著色，P 問題，有著 O(N²) 演算法，但不一定可以三著色。詳見 [Wiki](https://zh.wikipedia.org/zh-tw/%E5%9B%9B%E8%89%B2%E5%AE%9A%E7%90%86) 或 [演算法筆記](https://web.ntnu.edu.tw/~algo/Coloring.html)

???+note "k-Vertex Coloring"
	給一張 $n$ 點 $m$ 邊無向圖，問是否能 $k$-coloring，使每條邊兩端的顏色都不同
	
	??? note "思路"
		> k = 2
		
		二分圖判斷
		
		---
		
		> k = 3
		
		$O(m\times 2^n)$ 列舉第一種獨立集，剩下的二分圖
		
		---
		
		> k = 4
		
		令 $dp(S)=S$ 是否可以 2-colors 上色，建表複雜度 $O(m\times 2^n)$。列舉 $S$ 使得 $dp(S)$ 跟 $dp(V\setminus S)$ 都要是 true

???+note "Minimum Vertex Coloring  / Chromatic Number"
	給一張 $n$ 點 $m$ 邊無向圖，問最少塗幾種顏色可使每條邊兩端的顏色都不同
	
	??? note "思路"
		令 $dp(S)=S$ 最少多少 coloring，轉移式
		
		$$
		dp(S)=\min\limits_{T\in S} \{ dp(S \setminus T) + 1 \}
		$$
		
		其中 $T$ 要是獨立集，複雜度 $O(3^n)$
