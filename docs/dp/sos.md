- [CF 詳細講解](https://codeforces.com/blog/entry/45223)

## 介紹

??? note "問題"
	input: 每個 [0, n-1] 的 subset T 都有一個 weight w(T)
	
	output: f(S) = \sum_{T 是 S 的 subset} {w(T)}

dp(mask, i) : 只枚舉 mask 的 [0, i] 而其餘不動的總和

dp(S, i) : 看第 i 位選不選

- if (S & (1<<i)) dp(S, i) = dp(S, i-1) + dp(S ^ (1<<i), i-1)

- else dp(S, i) = dp(S, i-1)

## 例題1

???+note "[JOI 2018 p5](https://oj.uz/problem/view/JOI18_snake_escaping)"
	每個數自 $i$ 都有對應的 $w[i]$，給你 $Q$ 筆 $\text{query}$ 每筆會是一個 `0`, `1`, `?` 組成的二進制，`?` 可以是 `0` or `1`。問可以組出的 subset $s$ 的 $\sum w[s]$
	
	??? note "思路"
        - 最主要的觀察是鴿籠原理，$L\le 20$ 代表 `0` `1` `?` 最少的最大出現次數是 $6$
        - 所以看 `0` `1` `?` 哪個比較少就用哪個下手
        
        > calculate ?

        - 暴力枚舉 $\texttt{?}$ 要是什麼
        - $\begin{align}O(2^{\text{cnt[?]}})\end{align}$

        > calculate 0
        
        - 容斥原理的 superset
        - $\text{g}[10110]$ 包括 $\text{g}[11110],\text{g}[10111],\text{g}[11111]$
        - $1001?011$
        - 我要看的是那些 $0$，固定的要是那些 $0$
          - 所以問號會被我設為 $0$ 因為在這個情況 $0=$ **有**
        - 所以我需要減掉那些位置應該要是 $0$ 只是在做 subset 的時候有機會變 $1$ 的 $\texttt{bit}$
        - 做完容斥原理後我們應該會剩下 
          - $1001\color{red}0\color{white}11$
          - $1001\color{red}1\color{white}11$
        - 這代表什麼 $\texttt{?}$
          - 我們 $\text{g[]}$ 的 $\texttt{init}$ 也就是 $\text{g[0], g[1], g[2], g[3],}\ldots$ 等於 $\text{a[0], a[1], a[2], a[3],}\ldots$
        - 我們只是用 $0$ 來代表我們有計算到的 $\texttt{bit}$，本質是沒變的
        - 只是針對 $0$ 來代表 **有** 的概念
        - $O(2^\text{cnt[0]})$

        ```cpp linenums="1"
        for(int i = 0; i < L; ++i)
            for(int j = (1 << L) - 1;j >= 0; j--)  
                // 注意 j 從大到小 (對於 0 來說是從 null to all)
                if(((1<<i)&j)==0) g[j] += g[j^(1<<i)];
        ```

        > calculate 1

        -  容斥原理的 subset
        - $1001?011$
        - 我要看的是那些 $1$，固定的要是那些 $1$
          - 所以問號會被我設為 $1$ 因為在這個情況 $1=$ **有**
        - 所以我需要減掉那些位置應該要是 $1$ 只是在做 subset 的時候有機會變 $0$ 的 $\texttt{bit}$
        - 做完容斥原理後我們應該會剩下 
          - $1001\color{red}0\color{white}11$
          - $1001\color{red}1\color{white}11$
        - $O(2^\text{cnt[1]})$

        ```cpp linenums="1"
        for(int i = 0; i < L; ++i)
            for(int j = 0;j < (1 << L); ++j)
                if(((1 << i) & j) == 0) f[j] += f[j^(1<<i)];
        ```
        
        參考自 $\texttt{:}$ https://www.cnblogs.com/nightsky05/p/15563014.html

## 例題2

???+note "[CSES 1654](https://cses.fi/problemset/task/1654)"
    對於每筆 $\text{query}(x)$ 分別計算以下的 $y$ 的數量
    - $x \mid y = x$
    - $x\mathrel{\&} y=x$
    - $x \mathrel{\&} y \neq 0$ 
	
	??? note "思路"
	
		- [解答](https://github.com/yozen0405/c-projects/blob/main/markdown/1654.md)

## 例題3

???+note "[CF 1208F Bits And Pieces](https://codeforces.com/problemset/problem/1208/F)"
