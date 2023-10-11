???+note "[CF 1886 E - I Wanna be the Team Leader](https://codeforces.com/contest/1886/problem/E)"
    有 $m$ 個 projects，難度分別為 $b_1, \ldots ,b_m$，有 $n$ 個人，抗壓程度分別為 $a_1, \ldots ,a_n$。現在需要分組，滿足以下條件 :

    - 使每個 project 至少有一個人做
    - 每個人至多負責一個 project
    - 令負責第 $j$ 個 project 的人有 $k$ 個，則這些 $a_i$ 須滿足 $a_i\le\frac{b_j}{k}$

    $n\le 2\times 10^5, m\le 20,1\le a_i, b_i\le 10^9$
	
	??? note "思路"
        考慮將序列 $a$ 大到小 sort，每個 $b_i$ 挑的就會是一個 prefix，因為如果不是一個 prefix，那對於最後面的那格我們只在意前面選的數量，因此可將倒數第二格與前面的某個的 $b_i$ swap，還可以讓其他 $b_i$ 挑的 $a_i$ 的最小值變大，一直這樣做就會變好幾個連續區段。那問題就可以用 bitmask dp 解決，$dp[S]$ 代表 $S$ 目前 prefix 選到哪裡可使 $S$ 內的 $b_i$ 都滿足條件，轉移的話枚舉不在 $S$ 內的 $j$，看 $j$ 要延伸到哪裡，我們令 $j$ 支配的區段為  $[dp[S]+1,dp[S\cup j]]$，$dp[S\cup j]=nxt(j,dp[S])$。$nxt(j,i)$ 代表 $b_j$ 從 $i$ 開始往右選最少選到哪裡即可使 $b_j$ 滿足條件。



