打在二為座標平面

2023 JOI pB

副鍾魔鏡

2018 TOI p4

https://oi-wiki.org/basic/prefix-sum/

## 二維前綴和

```cpp linenums="1"
const int N = 505, M = 505;
int n, m, a[N][M], pre[N][M];

void build() {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + a[i][j];
        }
    }
}

int query(int i1, int j1, int i2, int j2) {
    int sum = 0;
    if (1 <= i1 && i1 <= n && 1 <= i2 && i2 <= n && 1 <= j1 && j1 <= m && 1 <= j2 && j2 <= m) {
        sum += pre[i2][j2] - pre[i2][j1 - 1] - pre[i1 - 1][j2] + pre[i1 - 1][j1 - 1];
    } else {
        return 0;
    }
    return sum;
}
```

## 二維差分

```cpp linenums="1"
const int N = 505, M = 505;
int n, m, a[N][M], pre[N][M];

void add(int i1, int j1, int i2, int j2, int x) {
    // 將左下角 (i1, j1) 右上角 (i2, j2) 的區域內都加上 x
    pre[i1][j1] += x;
    pre[i2 + 1][j1] -= x;
    pre[i1][j2 + 1] -= x;
    pre[i2 + 1][j2 + 1] += x;
}

void build() {
    // 差分完在做一次前綴和
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= m; j++) {
            pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + a[i][j];
        }
    }
    // 此時 pre[i][j] 就是 (i, j) 上面的數字
    // 而不是前綴序列
}
```

---

## 參考資料

- <https://zhuanlan.zhihu.com/p/439268614>