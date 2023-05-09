n 箱 m 球，箱異物同，每箱至少包含一個球，算出分配的方法數，請列出答案組合式子。
Hint: [stars and bars](https://en.wikipedia.org/wiki/Stars_and_bars_(combinatorics))

---

課堂上介紹了鏈著色問題，題目是將 n 個點的鏈塗上 m 種顏色，相鄰不可同色，求出方法數。

以下是上述問題的一個變化版本
將 n 個點鏈塗上 m 種顏色，相同顏色的兩個點至少要間隔 3 個節點，求出方法數

請設計一個時間複雜度至多 O(logn) 的演算法解決上述問題

---

有 n 個石頭，輸出有幾種方法可以把石頭分成奇數堆，且每堆的個數都不同，將答案 mod1000000007 輸出

例子：`n = 9`
`9 = 1 + 2 + 5`
`9 = 1 + 3 + 4`
`9 = 2 + 3 + 4`
`9 = 9`
總共 `4` 種方法

請設計一個時間複雜度至多 O(n^3) 的演算法解決上述問題

---

概述一下課堂上講的三種計算組合數計算方式各自的優劣和適用情況。

1. 預處理 階乘 和 階乘逆元，快速查詢 C(n,k)
2. 預處理二維組合數巴斯卡 DP 表，查表計算 C(n,k)
3. 直接用算式計算 C(n,k)

---

輸入兩個正整數 n, k，還有三個質數 p_1, p_2, p_3

令 M = p_1 \times p_2 \times p_3，我們希望輸出 C^{n}_{k} \mod M

已知 1 \leq n, k \leq 10^9 且 1\leq p_1, p_2, p_3 \leq 10^{6}

請設計一個時間複雜度至多 O(p_1 + p_2 + p_3) 的演算法解決上述問題

Hint: 中國剩餘定理

---

例題中的， [Atcoder 156D Bouquet](https://atcoder.jp/contests/abc156/tasks/abc156_d?lang=en)，因為 a, b 相對於 n 較小所以不一定要用到 `Lucas` 定理，若不使用 `Lucas` 定理的話可以怎麼解決此題？

請設計一個時間複雜度至多 O(max(a,b)) 的演算法解決上述問題

---

有一個 n×n 的棋盤格
一開始在 (1,1) 的位置，目標走到 (n,n) 的位置，每次只能將 x 加上 1 或是將 y 加上 1。

請用 n 表示有幾種不同的走法？

---

有一個 n×n 的棋盤格
一開始在 (1,1) 的位置，目標走到 (n,n) 的位置，每次只能將 x 加上 1 或是將 y 加上 1。

過程中不能經過 y = x - 1 的格子

請用 n 表示有幾種不同的走法？
Hint : 卡特蘭數

---

給 n 個介於 [1,M] 的整數，計算有幾對數字的 gcd 是二的冪次方

請設計一個時間複雜度至多 O(MlogM) 的演算法解決上述問題
Hint : 排容原理

---

給定四個數 x,y,z,len，請問同時滿足以下條件的 `tuple` (i,j,k) 有幾種 

1. (i+x,j+y,k+z) 可構成面積大於 0 的三角形的三邊長
2. i+j+k≤len

hint : 排容原理

---

- https://cses.fi/problemset/task/1715
- https://atcoder.jp/contests/abc172/tasks/abc172_e
- https://cses.fi/problemset/task/2187
- https://atcoder.jp/contests/abc156/tasks/abc156_e
- https://atcoder.jp/contests/abc205/tasks/abc205_e
- https://atcoder.jp/contests/abc207/tasks/abc207_e
- https://codeforces.com/problemset/problem/1444/B
- https://codeforces.com/problemset/problem/1342/E

