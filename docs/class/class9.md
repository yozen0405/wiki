有 n 個人排隊進入車站，每秒有一個人進入車廂的概率為 x，沒有人進車站機率為 1−x，求 t 秒後所有人都進入車站的機率，設計 DP 狀態並列出轉移求解此題。

---

給定一棵有根樹，從 Root 開始走，每次會以均等機率往任意一個 Child
走，走到葉節點就結束，算出期望值所需走的步數，設計 DP 狀態並列出轉移求解此題

---

有 n 個 x 面骰（數字分別是 1~x），各丟一次，求 n 個骰子中出現點數最大值的期望值
設計 DP 狀態並列出轉移求解此題。

---

有 n 種物品，每種東西被抽到的機率都是 1/n
求在 n 個東西中至少抽過 i 種不同物品至少一次的期望所需抽取次數。
設計 DP 狀態並列出轉移求解此題。

---

有 n 個物品，每個東西被抽到的機率都是 1/n
求在 k 次以內可以把 n 種東西都抽到至少一次的機率
設計 DP 狀態並列出轉移求解此題。

---

給一張 DAG，給起點終點，並且每個點都可以走到終點，每次會從目前所在點隨機選一條出邊走，問期望值要走幾步可到終點，設計 DP 狀態並列出轉移求解此題。

---

有 m 個人，每個人隨機分配到一個介於 [1,n] 的整數，每一個整數的機率都相等

已知 n=10^6
利用程式輔助計算最少需要幾個人，至少有兩個人分配到相同數字的機率會大於 1/2

---

C++ 有內建的隨機排序 random\_shuffle(arr.begin(), arr.end()) 可以把陣列隨機排序且使每個排列出現的機率均等，現在給定一個陣列，且可使用 rand() 生成一隨機數，設計一個時間複雜度至多 O(n) 的演算法，僅能額外使用常數記憶體空間，完成 random_shuffle 的功能，且需保證每個排列被抽到的機率均等。

---

輸入一個長度為 n 的整數陣列 a_1, a_2, ..., a_n

已知此陣列存在一個長度至少 n / 3 的子序列為等差數列。

設計一個時間複雜度至多 O(n) 的隨機演算法，有 99\% 正確找出該子序列，並分析該演算法所需運行的次數。

Hint:
從 1 ~ n 隨機抽 3 個數字 x, y, z，(y-x) 與 (z-x) 互質的機率可以視為常數。
隨著 n 愈大，這個機率會趨近於 $6/\pi^2 \simeq 0.6079$。
https://en.wikipedia.org/wiki/Coprime_integers#Probability_of_coprimality

---

輸入一個長度為 n 的座標點 (x1,y1),(x2,y2),...,(xn,yn)

已知存在一條線通過一半以上的點，出現 n/2 次以上

設計一個時間複雜度至多 O(n) 的隨機演算法，有 99% 正確找出該直線，並分析該演算法所需運行的次數。

---

- https://cses.fi/problemset/task/1725
- https://atcoder.jp/contests/dp/tasks/dp_j
- https://cses.fi/problemset/task/1728
- https://codingcompetitions.withgoogle.com/codejam/round/000000000087711b/0000000000acd29b
- https://codeforces.com/contest/442/problem/B
- https://codeforces.com/problemset/problem/1245/E
- https://codeforces.com/problemset/problem/1310/D