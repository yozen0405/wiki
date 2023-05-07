輸入一個長度 n 的正整數陣列 a[1],a[2],…,a[n]
另外有 q 次詢問，每次詢問為包含 tuple L,R,K
回答陣列  a[L],a[L+1],…,a[R] 區間中有幾個數不大於 K
請設計一個時間複雜度至多 O(nlogn+qlogn) 的演算法解決上述問題

hint: 對每個 query 按照 K 排序, 離線作法



---



輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{update}(i, v): 把 a[i] 改成 v(v>0)
\2. \text{query}(S): 找到最小的 x 滿足 a[1] + a[2] + \dots + a[x] > S，若不存在則回傳 -1

(2%) 請設計一個時間複雜度至多 O(n \log n + q \log n\log n) 的演算法解決上述問題
(1%) 請設計一個時間複雜度至多 O(n + q \log n) 的演算法解決上述問題

hint: 二分搜, 線段樹上二分搜



---

課堂所提到的 CSES - Josephus Problem II，本來的題目是每輪固定數 k 個。
若是每輪給定不同的 k 數值，請設計演算法解決此題，並分析演算法的複雜度。

---

輸入一個 n\times m 大小的二維陣列，每格有一個正整數，左上角是 A[1][1]，右下角是 A[n][m]。
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{row_add}(i, l, r, x): 將 A[i][l], A[i][l+1], \dots, A[i][r] 全部都加上 x
\2. \text{column_add}(i, l, r, x): 將 A[l][i], A[l+1][i], \dots, A[r][i] 全部都加上 x

請輸出修改過後的 A 陣列

請設計一個時間複雜度至多 O(n\times m + q) 的演算法解決上述問題

hint: 區間加值利用差分技巧變成單點加值

---



輸入一個 n\times m 大小的二維陣列，每格有一個正整數，左上角是 A[1][1]，右下角是 A[n][m]。
另外依序有 q 個動作，每次的動作可能是以下其中一種
\1. \text{query}(i, j): 輸出 A[i][j]
\2. \text{row_add}(i, l, r, x): 將 A[i][l], A[i][l+1], \dots, A[i][r] 全部都加上 x
\3. \text{column_add}(i, l, r, x): 將 A[l][i], A[l+1][i], \dots, A[r][i] 全部都加上 x

請設計一個時間複雜度至多 O(nm \log (n+m) + q\log(n+m)) 的演算法解決上述問題
hint: BIT, 第四題在線版本



---



輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{update}(i, v): 把 a[i] 改成 v
\2. \text{query}(l, r): 在 a[l], a[l+1], \dots, a[r] 內進行查詢，計算 a[l] - a[l+1] + a[l+2] - a[l+3] + a[l+4] - a[l+5] \dots + (-1)^{r-l}a[r]

請設計一個時間複雜度至多 O(n \log n + q \log n) 的演算法解決上述問題
hint: \text{query}(l, r) = (a[l] + a[l + 2] + ...) - (a[l + 1] + a[l + 3] + ...)



---



輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{update}(i, v): 把 a[i] 改成 v
\2. \text{query}(l, r): 在 a[l], a[l+1], \dots, a[r] 內進行查詢，找到第二大的的數字，若不存在則回傳 -1

請設計一個時間複雜度至多 O(n \log n + q \log n) 的演算法解決上述問題



---

輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{update}(i, v): 把 a[i] 改成 v
\2. \text{query}(l, r): 在 a[l], a[l+1], \dots, a[r] 內進行查詢，找到最大值出現的位置，若有多個位置都是最大值，回傳第一次出現的位置

請設計一個時間複雜度至多 O(n \log n + q \log n) 的演算法解決上述問題



---



輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{update}(i, v): 把 a[i] 改成 v
\2. \text{query}(l, r): 在 a[l], a[l+1], \dots, a[r] 內進行查詢，計算這個範圍的最大值總共出現幾次

請設計一個時間複雜度至多 O(n \log n + q \log n) 的演算法解決上述問題



---



輸入一個長度 n 的正整數陣列 a[1], a[2], \dots, a[n]
另外有 q 個動作，每次的動作可能是以下其中一種
\1. \text{query}(l, r): 判斷 a[l], a[l+1], \dots, a[r] 有沒有重複數字
\2. \text{update}(i, v): 把 a[i] 改成 v

請設計一個時間複雜度至多 O(n \log n + q \log n) 的演算法解決上述問題

hint: 維護每個 index 在陣列中右邊最近且數字相同的 index



---



- https://cses.fi/problemset/task/1650
- https://zerojudge.tw/ShowProblem?problemid=d794
- https://tioj.ck.tp.edu.tw/problems/1080
- https://zerojudge.tw/ShowProblem?problemid=g277
- https://cses.fi/problemset/task/2163
- https://cses.fi/problemset/task/1651
- https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ff43/0000000000337b4d