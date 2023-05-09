```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    if (0.1 + 0.2 == 0.3) {
        cout << "YES\n";
    } else {
        cout << "NO\n";
    }
}
```
問以上程式碼的輸出為何, 並說明其理由
請修正上述程式碼, 使他輸出預期的結果

---

給三個二維坐標, 每個數字都只到小數點第二位, 例如
`34.64 16.54`
`15.33 31.38`
`63.17 53.37`
輸出三個座標所形成的三角形面積。
如何在不使用 `double` 或 `float` 等小數資料型態下, 輸出三個座標所形成的三角形面積。

hint: 輸入的數字都只到小數點第二位, 計算三角形面積運用到的小數也只有除 2

---

平面上按照逆時鐘順序給定一個簡單多邊形的 n 個頂點座標，p[1],p[2],…,p[n]

請判斷這個簡單多邊形是否是凸多邊型

請設計一個時間複雜度至多 $O(n^2\log n)$ 的演算法解決上述問題

---

平面上給定一個凸多邊形的 n 個頂點座標，p[1],p[2],…,p[n]
但是這些點的順序目前的亂的 (不是按照逆時針順序排列)

請將這些點按照逆時針順序排列後輸出，y 值最大的點當第一個 （若有多個 y 最大的選 x 最小的）

請設計一個時間複雜度至多 O(nlogn) 的演算法解決上述問題

---

給平面上 n 個點座標

請計算最多有幾個點在同一條直線上

請設計一個時間複雜度至多 $O(n^2\log n)$ 的演算法解決上述問題

hint: 多個點要在同一直線上, 即這些點任選兩條線組成的直線斜率均一樣

---

給平面上 n 個點 p_1, p_2, \dots, p_n，
請計算有幾組 (i, j, k) 滿足

1. 1 \leq i < j < k \leq n
2. p_i, p_j, p_k 形成一個面積不為 0 的三角形

請設計一個時間複雜度至多 O(n^2 \log n) 的演算法解決上述問題

hint: 枚舉三角形的其中一條邊, 如果快速的求出可以和這條邊形成面積不為 0 的三角形個數, 注意同個三角形可能會被重複枚舉

---

給平面上 n 個紅點和 n 個藍點, 問存不存在一條直線, 可以將紅藍點分成兩半平面。請給出一個 O(nlogn) 的演算法

---

給平面上 n 個二維點 p_i = (x_i, y_i), 設計一個 O(n) 的演算法找到一個點 (x, y), 滿足 \sum{(|x_i - x| + |y_i - y|)} 最小

---

給一個 n 個點的簡單多邊形, 設計一個 O(n) 的演算法求出有幾個整數點在內部(在邊線上不算)

hint: [皮克定理](https://zh.wikipedia.org/wiki/皮克定理)

---

給三的二維點, 求一個半徑最小的圓使得三個點都在圓內(包含圓線上), 請設計一個 O(1) 的演算法

---

- https://leetcode.com/problems/check-if-it-is-a-straight-line/
- https://cses.fi/problemset/task/2191
- https://cses.fi/problemset/task/2190
- https://cses.fi/problemset/task/2192
- https://open.kattis.com/problems/robotprotection
- https://vjudge.net/problem/Aizu-CGL_2_D
- https://cses.fi/problemset/task/2195