有一個函數 F 的定義如下

1. F(0) = 1
2. F(1) = 1
3. F(n) = 2 \times F(n - 1) - F(n - 2) + 1 （當 n \geq 2）

給定 n 和一個質數 p，請輸出 F(n) \mod P 的結果

請設計一個時間複雜度至多 O(\log n) 的演算法解決上述問題

---

給 q 個範圍在 [1, M] 正整數，需輸出這 q 個數字質因數分解的結果
例：720 要輸出 720=2^4 \times 3^2 \times 5

請設計一個演算法解決上述問題，需要滿足

1. 時間複雜度至多 O(M \log M + q \log M)
2. 空間複雜度至多 O(M)

---

輸入一個長度 n 個範圍在 [1,M] 正整數陣列 a[0],a[1],…,a[n−1]
請從 a 陣列中選出 2 個數字，使選出的數字的最小公倍數 (LCM) 最大。

請設計一個時間複雜度至多 O(n+MlogM) 的演算法解決上述問題

---

輸入一個長度 n 個範圍在 [1,M] 正整數陣列 a[0],a[1],…,a[n−1]
請從 a 陣列中移除最少數字，使得剩餘所有數字的 GCD 比原陣列大。

請設計一個時間複雜度至多 O(M+nlogM+MlogM) 的演算法解決上述問題

---

給一個正整數 M

請計算有幾個合法的 (x,y) 數對滿足

1. 1≤x≤M
2. 1≤y≤M
3. x 和 y 互質，也就是 gcd(x,y)=1

請設計一個時間複雜度至多 O(MlogM) 的演算法解決上述問題

---

​	給定一正整數 N，求出 gcd(N, 1) + gcd(N, 2) + ... + gcd(N, N - 1) + gcd(N, N)。

---

在 mod 運算下，除法可以用模逆原（乘法反元素）來處理，請構造出一個例子

也就是說，請找到三個正整數 x, y, p，符合下列條件

1. x, y, p \geq 200
2. p 是質數
3. x\mod y = 0
4. (x\mod p) \mod (y \mod p) \neq 0

並找出 y 的模逆元

---

給一個正整數 M，請計算 [1,M] 內所有數字的因數個數總和，也就是要計算 d(1)+d(2)+⋯+d(M)，其中 d(x) 表示 x 有幾個因數。

請設計一個時間複雜度至多 O(\sqrt M) 的演算法解決上述問題

---

給定一個長度 n 陣列 a[0], a[1], \dots, a[n-1]，初始每數皆為 0，接下來有一系列兩種操作。

1. 給 (x, y) 將 a[x] 加 y
2. 給 (x, y) 輸出所有 i mod x = y 的 a[i] 值加總

請設計一個時間複雜度至多 O(q \sqrt{n}) 的演算法解決上述問題

---

給一個無向圖，包含 n 個節點和 m 條邊，每個節點上有一個數字，一開始每一個節點上的數字都是 0

接下有有 q 個操作，每次操作都是下面其中一種

1. \text{query}(x)： 輸出編號 x 的節點上面的數字
2. \text{add}(x)：把編號 x 的節點以及它的所有鄰居上面的數字都加上 1

請設計一個時間複雜度至多 O(q \sqrt{m}) 的演算法解決上述問題

---

- https://tioj.ck.tp.edu.tw/problems/1234
- https://codeforces.com/problemset/problem/1541/B
- https://atcoder.jp/contests/abc215/tasks/abc215_d
- https://zerojudge.tw/ShowProblem?problemid=d193
- https://codeforces.com/problemset/problem/1295/D
- https://codeforces.com/problemset/problem/1117/D
- https://cses.fi/problemset/task/1081
- https://cses.fi/problemset/task/2417
- https://codeforces.com/contest/1580/problem/C

