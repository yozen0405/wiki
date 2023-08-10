???+note "code"

	```cpp linenums="1"
	int par[21][maxn];
    int a[maxn];

    void build () {
        int lg = std::__lg(n);
        for (int i = 1; i <= n; i++) par[0][i] = a[i];
        for (int i = 1; (1 << i) <= n; i++) {
            for (int j = 1; j + (1 << i) - 1 <= n; j++) {
                par[i][j] = min (par[i - 1][j], par[i - 1][j + (1 << (i - 1))]);
            }
        }
    }

    int query (int l, int r) {
        int lg = std::__lg(r - l + 1);
        return min (par[lg][l], par[lg][r - (1 << lg) + 1]);
        // why (r - (1 << lg)) "加 1" ?
        // 一般在算從 x 往左過去包含 x 長度為 len 的左界為何
        // x - len + 1 , 發現其中有一個 + 1
        // 因為光 x - len 超界了(減太多加回去)
    }
    ```
	