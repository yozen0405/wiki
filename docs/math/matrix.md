## 矩陣快速冪

### 矩陣乘法

???+note "code"
    ```cpp linenums="1"
    using Matrix = vector<vector<int>>;
    Matrix operator* (const Matrix &A, const Matrix &B) {
        Matrix C(n, vector<int>(n));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % M;
                }
            }
        }
        return C;
    }
    ```

### 快速冪

???+note "code"
    ```cpp linenums="1"
    Matrix pow(Matrix A, int k) {
        // 不能 &A 注意!!!(會改到 A)
        int n = A.size();
        Matrix ret(n, vector<int>(n));
        for (int i = 0; i < n; i++) ret[i][i] = 1;
        // 初始化為單位矩陣
        while (k > 0) {
            if (k & 1) ret = ret * A;
            A = A * A;
            k >>= 1;
        }
        return ret;
    }
    ```

### 單位矩陣

$I=\begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$ 是單位矩陣，表示 $A_0$，類似整數的 $1$

- https://zh.wikipedia.org/zh-tw/%E5%96%AE%E4%BD%8D%E7%9F%A9%E9%99%A3

- 性質: $A \times I_n=A$ 
    - 其中 $I_n$ 為單位矩陣

- 第一 row 對應到第一項

- 第二 row 對應到第二項

- 第三 row 對應到第三項...

### 想法

#### 轉移

- Graph Path I
    - Matrix[d1]: A[i][j] 為從 i 走 d1 步到 j 的方法數
    - Matrix[d2]: B[i][j] 為從 i 走 d2 步到 j 的方法數
    - Matrix[d1 + d2]: C[i][j] 為從 i 走 d1 + d2 步到 j 的方法數
        - C[i][j] = (C[i][j] + A[i][k] * B[k][j]) % M
- Graph Path II
    - Matrix[d1] : A[i][j] 為從 i 走 d1 步到 j 的最短距離
    - Matrix[d2] : B[i][j] 為從 i 走 d2 步到 j 的最短距離
    - Matrix[d1 + d2] : C[i][j] = min(A[i][k] + B[k][j]) for k = 1~n (列舉中間點)
    - https://cses.fi/problemset/result/4903803/

#### code

???+note "[CSES - Fibonacci Numbers](https://cses.fi/problemset/task/1722/)"

    - 費式數列矩陣快速冪模板題
	
	??? note "code"
        ```cpp linenums="1"
        #include <bits/stdc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;
        using Matrix = vector<vector<int>>;
        using PQ = priority_queue<int, vector<int>, greater<int>>;

        const int INF = 2e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        int n, x;

        Matrix operator* (const Matrix &A, const Matrix &B) {
            Matrix C = Matrix (n, vector<int>(n, 0));

            for (int i = 0; i < n; i++) { // A row
                for (int j = 0; j < (int) B[0].size(); j++) { // B col
                    for (int k = 0; k < n; k++) {
                        C[i][j] += A[i][k] * B[k][j];
                        C[i][j] %= M;
                    }
                }
            }

            return C;
        }

        Matrix pow (Matrix A, int b) {
            Matrix C = Matrix (n, vector<int>(n, 0));
            for (int i = 0; i < n; i++) C[i][i] = 1;

            while (b != 0) {
                if (b & 1) C = C * A;
                A = A * A;

                b >>= 1;
            }

            return C;
        }

        void init () {
            n = 3;
            cin >> x;
        }

        void solve () {
            Matrix A = {{1, 1, 0}, {1, 0, 0}, {0, 1, 0}};
            Matrix B = {{1}, {1}, {0}};

            if (x == 0) cout << 0, exit(0);
            if (x == 1) cout << 1, exit(0);
            if (x == 2) cout << 1, exit(0);

            Matrix C = pow (A, x - 2) * B;
            cout << C[0][0] << "\n";
        } 

        signed main() {
            // ios::sync_with_stdio(0);
            // cin.tie(0);
            int t = 1;
            //cin >> t;
            while (t--) {
                init();
                solve();
            }
        } 
        ```

## 矩陣壓縮

???+note "[CF 223 C. Partial Sums](https://codeforces.com/contest/223/problem/C)"
	有一個長度為 $n$ 的數組 $a_1,\ldots ,a_n$，每次用當前 $a$ 的前綴和數組代替$a$，求執行 $k$ 次操作後的數組
	
	$n\le 2000,k,a_i\le 10^9$
	
	??? note "思路"
		列出轉移式
	
		$$\begin{pmatrix} a[k][1] \\ a[k][2] \\ a[k][3] \\ a[k][4] \end{pmatrix}\times \begin{pmatrix} 1 & 1 & 1 & 1 \\ 0 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & 1 \end{pmatrix} = \begin{pmatrix} a[k + 1][1] \\ a[k + 1][2] \\ a[k + 1][3] \\ a[k + 1][4] \end{pmatrix}$$
		
		$$\begin{pmatrix} a_1 \\ a_2 \\ a_3 \\ a_4 \end{pmatrix}\times \begin{pmatrix} 1 & 1 & 1 & 1 \\ 0 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & 1 \end{pmatrix}^k = \begin{pmatrix} a[k][1] \\ a[k][2] \\ a[k][3] \\ a[k][4] \end{pmatrix}$$
		
		但矩陣乘法要 $O(n^3)$，我們必須想辦法將他押到 $O(n^2)$，我們將轉移矩陣的 $k=1,2,\ldots$ 次方列出來看看
		
		$$\begin{pmatrix} 1 & 1 & 1 & 1 \\ 0 & 1 & 1 & 1 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 0 & 1 \end{pmatrix}\Rightarrow\begin{pmatrix} 1 & 2 & 3 & 4 \\ 0 & 1 & 2 & 3 \\ 0 & 0 & 1 & 2 \\ 0 & 0 & 0 & 1 \end{pmatrix}\Rightarrow \begin{pmatrix} 1 & 4 & 10 & 20 \\ 0 & 1 & 4 & 10 \\ 0 & 0 & 1 & 4 \\ 0 & 0 & 0 & 1 \end{pmatrix}\Rightarrow \cdots$$
		
		我們會發現我們只需要用矩陣的第一個 row 即可求得整個矩陣，在實現乘法時即儲存第一個 row 並修改矩陣乘法的 code 即可
		
		> 參考 : <https://blog.csdn.net/sdz20172133/article/details/97273071>
	
## 高斯消去

- <https://github.com/NCTU-PCCA/NCTU_Fox/blob/master/codebook/Math/GaussElimination.cpp>
