定義一個遞迴式 `F`

1. 當 0 \le n < 3, F(n) = 1
2. 當 n \ge 3, F(n) = 3 * F(n - 1) - F(n - 2)

給定 n 和一個質數 P，輸出 F(n) \mod P

請設計一個時間複雜度至多 O(\log n) 的演算法解決上述問題

Hint: 矩陣快速冪

---

定義一個遞迴式 `F`

1. 當 0 \le n < 2, F(n) = n
2. 當 n \ge 2, F(n) = F(n - 1) + F(n - 2) + n^2 + 2n + 1

給定 n 和一個質數 P，輸出 F(n) \mod P

請設計一個時間複雜度至多 O(\log n) 的演算法解決上述問題

Hint: 矩陣快速冪

---

下面是一個 dp 轉移關係，他列出了下列狀態轉移式。給定一個長度為 n 的陣列 a

$$
dp[i] = \max_{0\leq j < i } \left\{ a[i] + \max_{l(j)\leq k \leq r(i) } dp[k] \right\}
$$

目前的狀態數量為 O(n), 如果暴力的轉移會需要 O(n^2) 的時間, 因此總複雜度為 O(n^3)

1. 假設 l(j) 和 r(j) 沒有任何性質, 請說明轉移如何做到至少 O(\lg n) 使得總複雜度為 O(n\lg n)
2. 假設 l(j) 和 r(j) 皆隨著 j 遞增, 請說明轉移如何做到均攤 O(1) 使得總複雜度為 O(n)

---

單調隊列課堂中提到的 dp 轉移通式為 $dp[i] = \mathop{max}\limits_{l(i) \le j < i}{A[j]}$ 並強調 `l` 函數要是**非遞減函數**，為何需要此條件要求，未滿足會產生甚麼錯誤？

---

已知 arr 為一個長度為 n 的正整數序列 (1≤arr[i]≤10^9)，考慮以下函數。
```cpp
int func_slow(const vector<int> &arr) {
    int n = arr.size();
    int ans = 0;
    for (int i = 0; i < n; i++) {
        int maxV = 0, cur = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] >= maxV) {
                cur++;
                maxV = arr[j];
            }
        }
        ans = max(ans, cur);
    }
    return ans;
}
```
1. 分析上述程式碼的複雜度。
2. 請給出一個改良過後的演算法可以在 O(n) 時間內完成 (Hint: 尋找其單調性質)

---

給一個長度為 n 的正整數序列 A[1], A[2], \dots, A[n]，和整數 k

求出有多少個長度 n 的正整數序列 V[1], V[2], \dots, V[n] 滿足

1. V[i] \leq A[i]
2. \mathop{\sum_{i=1}^{v.size}} V_i \leq k

請設計一個時間複雜度至多 O(nk) 的演算法求出合法的序列數量

Hint: 前綴和優化

---

給一個長度為 n 的正整數序列 A[1],A[2],…,A[n]，找出最長子序列 S 滿足每項皆為前一項 +1
請設計一個時間複雜度至多 O(nlogn) 的演算法
Hint: map優化

---

給一個長度為 n 的正整數序列 A[1], A[2], \dots, A[n]，求出有多少組 `pair` (i, j) 滿足以下條件

1. 1 \le i < j \le n
2. A[i] < A[j]

請設計一個時間複雜度至多 O(n \log n) 的演算法求出合法的 `pair` 數量
Hint: 資料結構優化

---

給一個長度為 n 的正整數序列 A[1], A[2], \dots, A[n]，求出有多少組 `tuple` (i, j, k) 滿足以下條件

1. 1 \le i < j < k \le n
2. A[i] < A[j] < A[i] * 2
3. A[j] < A[k] < A[j] * 2

請設計一個時間複雜度至多 O(n \log n) 的演算法求出合法的 `tuple` 數量
Hint: 資料結構優化

---

給一個長度為 n 的正整數序列 A[1],A[2],…,A[n]，和一個正整數 k

請找出一個總和最大的子序列，滿足 A序列中每連續 k 項中至少要有一個沒有選

請設計一個時間複雜度至多 O(n) 的演算法解決上述問題

Hint: 前綴和優化、單調隊列優化

---

- https://codeforces.com/contest/222/problem/E
- https://atcoder.jp/contests/dp/tasks/dp_m
- https://leetcode.com/problems/odd-even-jump/
- https://codeforces.com/problemset/problem/1313/c2
- https://codeforces.com/problemset/problem/474/E
- https://codeforces.com/problemset/problem/1077/F2
- https://codeforces.com/problemset/problem/1000/F
- 