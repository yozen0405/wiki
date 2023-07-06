## 摩爾投票算法

<font style='font-size:12px'>:octicons-dash-16: <font style='font-size:11px'>參考自 [abc864197532](https://abc864197532.github.io/2021/02/07/tioj-2140/)</font></font>

假設現在站在擂台上的數字為 $x$，他的血量為 $h$，則

 1.若出現的數字為 $x$，則他的血量 +1

 2.否則他的血量 -1，若血量為 0 就換一個數站上去

最後還在擂台上的數字才有可能是絕對多數，所以只要再花 $O(n)$ 的時間判斷他是不是絕對多數即可

??? note "code"
    ```cpp linenums="1"
    vector <int> a(n);
    int x = -1, h = 0;
    for (int i = 0; i < n; ++i) {
        if (x == a[i]) {
            h++;
        } else {
            if (h == 0) x = a[i], h = 1;
            else h--; 
        }
    }
    int cnt = 0;
    for (int i = 0; i < n; ++i) {
        if (a[i] == x) cnt++;
    }
    if (cnt < n / 2 + 1) cout << -1 << endl;
    else cout << x << endl;
    ```

???+note "[LeetCode 169. Majority Element](https://leetcode.com/problems/majority-element/)"
	給一個長度為 $n$ 的陣列，輸出出現超過 $\lfloor n/2\rfloor$ 次的數字
	
	$n\le 5\times 10^4$

???+note "[LeetCode 229. Majority Element II](https://leetcode.com/problems/majority-element-ii/)"
	給一個長度為 $n$ 的陣列，輸出所有出現超過 $\lfloor n/3\rfloor$ 次的數字
	
	$n\le 5\times 10^4$

???+note "[資芽 OJ 794 — 區間絕對眾數](https://neoj.sprout.tw/problem/794/)"

    輸入一個長度為 $N$ 的正整數序列 $a_1, \ldots, a_N$，接下來有 $Q$ 筆詢問。
    
    每筆詢問輸入 $l_i, r_i$，輸出區間 $[l_i, r_i]$ 的絕對眾數，若不存在請輸出 $0$。
    
    $N, Q \leq 5 \times 10^5, 1 \leq a_i \leq 5 \times 10^5$

Majority Voting
-  https://codeforces.com/blog/entry/89880

- https://zh.wikipedia.org/zh-tw/多数投票算法