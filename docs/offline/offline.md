### CSES 1734

???+note "[Distinct Values Queries](https://cses.fi/problemset/task/1734)"
    給你 $a_1,...,a_n$，有 $q$ 個查詢
    
    - $a_i,...,a_j$ 之間有幾種數字

### 類似題
???+note "[CF 1000F](https://codeforces.com/problemset/problem/1000/F)"
    給你 $a_1,...,a_n$，有 $q$ 個查詢
    
    - 隨便輸出一個 $a_l,...,a_r$  之間只出現一次的數
    
    ??? note "思路"
        - 我們假設 $a_i$ 上一次出現的地方是 $a_j$ 其中 $i<j$
    
        - $\texttt{seg}[i]:\begin{cases} j & \texttt{if j exist} \\ -\texttt{INF} &\texttt{otherwise}\end{cases}$
    
        - 對於 $\texttt{query(l,r)}$ 只要去 $\texttt{query_min(l,r)}$ 檢查是否比 $l$ 小即可
    
        - 記得在 $\texttt{query_min(l,r)}$ 要存任意一個符合答案的數

### 自創題
???+note "自創題"
    給你 $a_1,...,a_n$，和一個數 $x$，有 $q$ 個查詢
    
    ??? note "思路"
        - 輸出在 $a_l,..., a_r$ 之間存在幾個只出現一次的數
    
        - 只要將 $\texttt{CSES}$ 上面那題
    
        - 把查詢 $[l, r]$ 按照 $l$ 從大到小處理
    
        - 對於在位置 $l$ 後面的數字 $x$
    
        - 如果是第一次出現設定成 $+1$
    
        - 如果是第二次出現設定成 $-1$
## Atcoder abc250_e
???+note "[E - Prefix Equality](https://atcoder.jp/contests/abc250/tasks/abc250_e)"
    給你 $a_1,...,a_n$ 與 $b_1,...,b_n$，有 $q$ 個查詢
    
    - 問 $\texttt{set}(a_1,...,a_i)$ 和 $\texttt{set}(b_1,...,b_j)$ 是否相同
    
    ??? note "思路"
        - 暴力作法無法通過
    
        - $\texttt{seg}[i]$ 存 $a_i$ 這個數字在 $b$ 陣列首次出現的 $\texttt{index}$
    
        - $\text{query}$ 就直接看 $\texttt{seg.query_max(1,i)}$ 是否比 $\texttt{j}$ 小，比 $\texttt{j}$ 小代表是相同的

### 類似題
???+note "[选数异或](http://oj.ecustacm.cn/problem.php?id=2024)"
    給你 $a_1,...,a_n$，和一個數 $x$，有 $q$ 個查詢
    
    - 問 $[l,r]$ 中是否有兩個數 $\texttt{XOR}$ 起來等於 $x$
    
    ??? note "思路"
        - [解答](https://blog.csdn.net/m0_52398496/article/details/124608594?spm=1001.2014.3001.5502)

## RMQ
???+note "[Static Range Minimum Queries](https://cses.fi/problemset/task/1647)"
    給你 $a_1,...,a_n$，有 $q$ 個查詢
    
    - 問 $a_l,...,a_r$ 的最小值
    
    註$\texttt{:}$ 使用單調 $\texttt{stack}$ 來解決此問題
    
    ??? note "思路"
        - 依照 $r$ $\texttt{sort}$
    
        - 對於相同 $r$ 的 $\texttt{query}$，按照 $l$ 去 $\texttt{lower_bound}$
    
        - 因為單調 $\texttt{stack}$ 是遞增的，所以找到第一個 $idx \ge l$  一定是最好的 $\texttt{min}$
        
        - 小的一定都會把大的砍掉

## CSES  2416
???+note "[Increasing Array Queries](https://cses.fi/problemset/task/2416)"
    給你 $a_1,...,a_n$，有 $q$ 個查詢
    
    - 如果你一個步驟可以 $\texttt{modify}(a[i],+1)$ 
    
    - 請問你最少需要幾個步驟可以使 $a_l,...,a_r$ 非嚴格遞增

- CSES - Pizzeria Queries

- CSES - Polynomial Queries