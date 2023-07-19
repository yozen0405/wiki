依序輸入 $n$ 個整數 $a_1,a_2,\ldots,a_n$，每輸入一個整數就要輸出目前已經輸入的數字的中位數。
例如輸入為 $[1,7,4,2,5,9]$，輸出依序應為 $[1,4,4,3,4,4.5]$。

設計一個時間複雜度為 $O(n \log n)$ 的演算法解決此問題。

hint: rank-tree

---

依序輸入 $n$ 個整數 $a_1,a_2,\ldots,a_n$，每輸入一個整數就要輸出目前左邊有幾個數字比自己小
例如輸入為 $[1,7,4,2,5,9]$，輸出依序應為 $[0,1,1,1,3,5]$。

設計一個時間複雜度為 $O(n \log n)$ 的演算法解決此問題。

hint: rank-tree

---

輸入一個長度 $n$ 的正整數陣列 $a[0],a[1],\ldots ,a[n−1]$
另外有 $q$ 個查詢，每個查詢 $\text{query}(l,r)$ 要在 $a[l],\ldots ,a[r]$ 選一些數字，選出的數字不能是相鄰的（選了$a[5]$，就不能選 $a[6]$），目標是讓總和遇大愈好

請設計一個時間複雜度至多 $O(n \log n+q\log n)$ 的演算法解決上述問題

hint:
```cpp
Node{
  int ans;`
  int sum[2][2];  
  // sum[u][v] 
  // u:表示最左邊的的有沒有選
  // v:表示最右邊的的有沒有選
};
```

---

線段樹的區間修改通常須用到 lazy tag，其中少數一種例外為區間加值單點查詢，請問該如何不使用 lasy tag 完成此操作?

---

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{div}(l, r):$  將 $a[l], \dots, a[r]$ 每一項都除以二無條件捨去
2. $\text{query}(l, r):$ 計算 $a[l], \dots, a[r]$ 的總和

已知 $a[i] \leq m$

請設計一個時間複雜度至多 $O(n \log m)$ 的演算法解決上述問題


hint: 每個數字只要被除 $(1+\log m)$ 次，就會變成 $0$

---

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{update}(x, v) :$ 將 $a[x]$ 改成 $v$
2. $\text{query}(l, r):$ 計算 $1\times a[l] + 2\times a[l+1] + 3 \times a[l+2] + \dots + (r-l+1) \times a[r]$

請設計一個時間複雜度至多 $O(n \log n + q \log n)$ 的演算法解決上述問題

---

給一個正整數 $w$
輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{update}(x, v) :$ 將 $a[x]$ 改成 $v$
2. $\text{query}(l, r):$ 計算 $w^1 \times a[l] + w^2 \times a[l+1] + w^3 \times a[l+2] + \dots + w^{r-l+1} \times a[r]$

請設計一個時間複雜度至多 $O(n \log n + q \log n)$ 的演算法解決上述問題

note: 
本題的查詢功能可以應用在 rolling hash 演算法，用來做到動態判斷字串是否相等，或是動態判斷區間是否為回文

---

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{update}(l, r, v) :$ 將 $a[l] \dots a[r]$ 都加上 $v$
2. $\text{query}:$

  找三個數字 $i, j, k$，滿足 $0\leq i \leq j \leq k \leq n-1$
  目標讓 $a[i] - 2\times a[j] + a[k]$ 最大化

請設計一個時間複雜度至多 $O(n \log n + q \log n)$ 的演算法解決上述問題

---

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{update}(l, r, v) :$ 將 $a[l] \dots a[r]$ 都加上 $v$

	時間複雜度 $O(\log n)$

2. $\text{query}:$

	找兩個數字 $i, j$，滿足 $0\leq i \leq j \leq n-1$，目標讓 $-1 \times a[i] + a[j]$ 最大化
	
	時間複雜度 $O(\log n)$

假設你已經完成了上面功能的資料結構
如何利用上面的資料結構完成下面的問題?

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個動作，每次的動作可能是以下其中一種

1. $\text{update}(x, v) :$ 將 $a[x]$ 加上 $v$
2. $\text{query}(l, r):$ 查詢 $a[l] \dots a[r]$ 的最大連續和 

請設計一個時間複雜度至多 $O(n \log n + q \log n)$ 的演算法解決上述問題

---

輸入一個長度 $n$ 的正整數陣列 $a[0], a[1], \dots, a[n-1]$
另外有 $q$ 個查詢，每個查詢 $\text{query}(l, r, k)$ 要輸出 $a[l], \dots, a[r]$ 有幾個數字小於 $k$。

請設計一個時間複雜度至多 $O(n \log n + q \log q)$ 的演算法解決上述問題

hint: 離線處理，把查詢按照 $k$ 小到大處理

---

- https://codeforces.com/edu/course/2/lesson/4/2/practice/contest/273278/problem/B
- https://codeforces.com/edu/course/2/lesson/4/2/practice/contest/273278/problem/A
- https://codeforces.com/edu/course/2/lesson/5/2/practice/contest/279653/problem/A
- https://codeforces.com/edu/course/2/lesson/5/2/practice/contest/279653/problem/F
- https://codeforces.com/contest/597/problem/C
- https://cses.fi/problemset/task/2416
- https://tioj.ck.tp.edu.tw/problems/1208
- https://tioj.ck.tp.edu.tw/problems/1224

