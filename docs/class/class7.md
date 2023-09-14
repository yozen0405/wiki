給一個長度為 n 的正整數陣列 a[1],a[2],…,a[n], 
對於陣列中的每一個數值, 找一個他右邊數值比他小並最近的數字位置, 請使用單調 stack 解決此問題，請說明要使用遞增 or 遞減的 stack 並列出對以下測資處理至每一項時 stack 的內容物，並列出答案。
`array = {3, 2, 1, 2, 6, 5, 2, 3, 2, 7, 2, 10}`

---

給一個長度為 n 的正整數陣列 a[1],a[2],…,a[n], 求出每個連續 k 項子陣列的最小值, 請使用單調隊列解決此問題，請說明要使用遞增 or 遞減的 queue 並列出對以下測資處理至每一項時 queue 的內容物，並列出答案。
例如
`array = {1, 2, 1, 2, 6, 8, 2, 5, 4, 7, 8, 10}, k = 5`

---

一開始有 n 個東西，利用啟發式合併演算法執行 n−1 次集合合併會將所以東西合併到一個集合。
分析 Best Case 和 Worst Case 的輸入所需要的搬運次數，並概述這兩種情況的合併流程？

---

給一個長度為 n 的正整數序列 a[1], a[2], \dots, a[n]，
有 q 筆詢問, 每次詢問陣列 [a, b] 這個區間中出現最多次的數字的出現次數

請設計一個時間複雜度至多 O(n \sqrt n + Q \sqrt n) 的演算法解決上述問題

hint: 莫隊

---

假設 N=16, 請設計出一系列 L,R 區間詢問，使得其為莫隊的 worst case, 即兩筆詢問之間 pointer 移動次數皆為 O(\sqrt n) 規模

---

給一個長度為 n 的正整數序列 a[1], a[2], \dots, a[n]，
有 q 筆詢問, 每次詢問陣列 [a, b] 這個區間中有幾種不同的數字

請設計一個時間複雜度至多 O(n\log n + q\log n) 的演算法解決上述問題

hint: 離線，將詢問排序，線段樹

---

請用單調 stack 解決不含修改的區間最大值問題，時間複雜度至多 O(nlogn)。

hint: 離線，將詢問排序，二分搜

---

請使用兩個 stack 來模擬一個 queue, 並且模擬出來的 queue 的 push 和 pop 操作都要時間複雜度均攤 O(1)

---

給定一 1~n 的 permutation，多次詢問給左右區間，問這區間有幾對數滿足 x mod y = 0。
設計一個時間複雜度至多 O(n\log^2 n + q\log n) 的演算法解決上述問題
hint: 離線，將詢問排序，線段樹

---

給一個長度為 n 的正整數陣列 a[1], a[2], \dots, a[n]。
找到兩個 index i, j 滿足 i\leq j
目標是讓 (j-i+1) \times \min(a[i], a[j]) 最大化

請設計一個時間複雜度至多 O(n) 的演算法解決上述問題

hint: 單調 stack，若連續三個數字 x, y, z 滿足 y 是三個數字中最小的，某些元素會是確定用不到的。

---

- https://atcoder.jp/contests/abc247/tasks/abc247_d
- https://leetcode.com/problems/largest-rectangle-in-histogram/
- https://zerojudge.tw/ShowProblem?problemid=c528
- https://atcoder.jp/contests/abc183/tasks/abc183_f
- https://codeforces.com/contest/1637/problem/E
- https://codeforces.com/problemset/problem/600/E
- https://codeforces.com/contest/915/problem/E

- https://tioj.ck.tp.edu.tw/contests/81/problems/2161