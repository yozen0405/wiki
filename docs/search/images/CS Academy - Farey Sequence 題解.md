# CS Academy - Farey Sequence 題解

## 作法

考慮小數點二分搜，我們去二分搜到剛好有 $k$ 個比 $x$ 小（最小的 $x$ 滿足 $\le x$ 的分數恰為 $k$ ）

至於如何檢查，對於一個固定的分母 $j$，有多少個分子 $i$ 會小於 $x$ 呢 ? 
$$
\displaystyle \frac{i}{j}\le x  \Rightarrow i\le \lfloor x\times j \rfloor
$$
答案是 $\displaystyle \frac{1}{j}\sim \frac{\lfloor x\times j \rfloor}{j}$，也就是 $\lfloor x\times j \rfloor$ 個，但如果枚舉分母，都這樣會算的話會算到重複的，例如 $\frac{2}{4}$ 會在分母為 $2$ 與 $4$ 各被算過一次，很明顯就是因數的問題，所以我們列出
$$
dp[j]=\lfloor x\times j \rfloor - \sum \limits_{c\mid j}dp[c]
$$
具體實作類似篩法，如下

```cpp
int dp[N];
bool check(double x) {
    for (int i = 1; i <= n; i++) {
        dp[i] = 0;
    }
    int sum = 0;
    for (int j = 2; j <= n; j++) {
        dp[i] += x * j;
        for (int i = j + j; i <= n; i += j) {
            dp[i] -= dp[j];
        }
        sum += dp[j];
    }
    return sum >= k;
}
```

最後，要輸出分數答案時，找比 $x$ 小裡面最大是那個分數。這可以枚舉分母 $j$，以 $j$ 為分母第一個小於等於 $x$ 的分子就是 $\lfloor x\times j\rfloor$，對於每個分母算出來的取 $i/j$ 最大的即可

---

## 另法

另一種想法也類似，我們觀察到，Farey 序列裡面相鄰兩項的差大於 $\frac{1}{n\times n}$[^1]，所以我們可以 $\frac{1}{n\times n}$ 為一單位將序列隔開。

![](https://i.imgur.com/JoG2Znk.png)

所以我們可以去二分搜 $x$ 滿足小於等於 $\frac{x}{n\times n}$ 的分數恰為 $k$ 個，檢查的話一樣推倒
$$
\displaystyle \frac{i}{j}\le \frac{x}{n\times n}  \Rightarrow i\le \lfloor \frac{x}{n\times n} \times j \rfloor
$$
也一樣用類篩法實作，最後要輸出答案時，找 $\frac{i}{j}$ 滿足 $\frac{i}{j}$ 恰好在 $\frac{x-1}{n\times n}$ 與 $\frac{x}{n\times n}$ 之間。實作上一樣枚舉分母 $j$，看以分母為 $j$，第一個小於等於 $\frac{x}{n\times n}$ 的 $\frac{i}{j}$ （$i=\lfloor \frac{x}{n\times n} \times j \rfloor$）是否介於 $\frac{x-1}{n\times n}$ 與 $\frac{x}{n\times n}$ 之間。

[^1]: 根據 Farey 序列的性質，相鄰兩項 $\frac{q_1}{p_1},\frac{q_2}{p_2}$ 的差 $=\frac{1}{p_1\times p_2}$，證明見 [Wiki](https://en.wikipedia.org/wiki/Farey_sequence#Farey_neighbours)

