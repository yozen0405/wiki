課堂上提到的用 LIS 優化 LCS 的方法，若兩字串內有重複字元要如何更改算法並試著分析其複雜度。
嘗試以 字串 1 : “eacbdbae”，字串 2 : “debaabca”為例子說明如何轉成 LIS。

---

分治法例題1 `NCTUOJ 670. Maximum Rectangle` 中，題目有假設紅色點和藍色點會被一條直線分開，若題目改為不限制紅色和藍色點的位置，要如何修改演算法？並說明其作法和複雜度。

---

試著證明課堂講義分治法例題 `codeforces 868 F. Yet Another Minimization Problem` 為何可以用分治法優化，並分析他的決策點單調性質。

---

在斜率優化的問題中，我們討論到了他的 dp 轉移通式 dp[i] = \mathop{max}\limits_{0 \le j < i}\{a(j) * f(i) + b(j)\}。
考慮一個特別的情況
\- b(j) = 0
\- a(j) = dp[j]
\- f(i) = i

轉移式變為 dp[i] = \mathop{max}\limits_{0 \le j < i}(dp[j] * i)，請問是否有什麼特殊性質可以優化？請提出推導過程並分析其複雜度。

---

有一個長度為 2^n 的陣列 F[0], F[1], ..., F[2^n - 1]，要在 O(n \times 2^n) 的時間內求出一個長度為 2^n 的陣列 G，其中 G[i] = \mathop{max}\limits_{s \subseteq i} F[s]。

---

試問 input mask 為 $10101_{(2)}$ 時，程式依序會輸出哪些 submask，並根據輸出結果說明以下程式為何可以輸出所有給定 mask 的所有 submask

```cpp
void print_all_submask(int mask) {
    for (int S = mask; ; S = (S - 1) & mask) {
        cout << S << '\n';
        if (S == 0)
            break;
    }
}
```

---

給定一個 array 包含 $n$ 個數，每個數至多為 $2^k$，計算有幾個 `pair` (i, j) 滿足 $a_i$ | $a_j$ = 0，設計一演算法在 $O(3^k)$ 算出答案。

---

請設計一個時間複雜度 O(n) 的演算法解決此問題

hint: 斜率優化

---

有 $n$ 個人和，$m$ 個物品，每人至多喜歡 $p$ 個物品，選一個物品的集合，使得集合物品數量最大且至少 $⌊\frac{n}{2}⌋$ 人同時喜歡集合中所有物品。

設計一隨機演算法在 $\texttt{iteration}\times O(3^p)$ 算出答案。

---

- https://codeforces.com/problemset/problem/486/E
- https://codeforces.com/problemset/problem/1442/D
- https://codeforces.com/problemset/problem/1425/B
- https://codeforces.com/problemset/problem/868/F
- https://codeforces.com/problemset/problem/1083/E
- https://codeforces.com/problemset/problem/1208/F
- https://atcoder.jp/contests/dp/tasks/dp_u



