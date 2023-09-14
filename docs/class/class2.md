輸入一棵樹，根節點編號為 1，樹上每點有點權，點權的值只會是 [1,5] 內的整數

計算每個點為根的子樹有幾種不同點權

請給出一個時間複雜度至多 O(n) 的演算法解決此問題

---

輸入一棵樹，根節點編號為 1，接下來有 m 個操作，操作為兩個整數 x y 代表將以 x 為根的子樹的值都加 y，最後輸出每個點的權值

請給出一個時間複雜度至多 O(n) 的演算法解決此問題

---

試說明連通圖經過 BCC 縮點，為何一定會形成一棵 Tree。

---

試說明有向圖經過 SCC 縮點，為何一定會形成一個 DAG(有向無環圖)。

---
請對下圖執行 Tarjan 演算法，計算出每個點的 low 和 dfn

![](https://imgur.com/elDx5LP)

(演算法執行過程優先走數字小的 neighbor)

---

呈上題，利用上題的 low 和 dfn 資訊找出所有橋和割點，並說明判斷依據

---

給一張有向圖 G，包含 n 個節點和 m 條邊，一個 walk 表示可在圖上行走，可能重複經過同一個點或邊。

輸出一個 walk 能經過最多不同的點的數量，請給出一個時間複雜度至多 O(n+m) 的演算法解決此問題

---

有 n 個變數 v_1, v_2, \dots, v_n，每個變數可以指定成 \{ 1, 2\} 其中一個。
有 2 種類型的限制，每種限制給兩 index x, y
1. v_x \neq v_y
2. v_x + v_y > 2
請對此題完整列出所有 2-SAT 式子
1. 初始條件
2. 每種限制須增加的式子

---

請敘述 2-SAT 演算法中，從 SCC 縮點完結果到構造出一組答案的詳細流程。

並說明為照此流程找到的答案一定為合法解
1. 為何 x 和 not x 會恰好挑到一個
2. 為何不會挑到有衝突的答案

---

- https://codeforces.com/problemset/problem/862/B
- https://tioj.ck.tp.edu.tw/problems/1687
- https://tioj.ck.tp.edu.tw/problems/1137
- https://tioj.ck.tp.edu.tw/problems/1879
- https://cses.fi/problemset/task/1683
- https://cses.fi/problemset/task/1136
- https://codeforces.com/contest/1143/problem/E
- https://tioj.ck.tp.edu.tw/problems/1683
- https://tioj.ck.tp.edu.tw/problems/2196

