## LCS 介紹

???+note "LCS"
	輸入兩個字串 s, t，長度分別是 n, m，找到 s, t 的最長共同子序列長度
	
考慮兩個字串的最後一個字元是否可以配對，dp(i, j) 表示只看 s[0~i] 與 t[0~j] 的 LCS 長度，i, j 一定要配在一起，那這樣轉移就會是去枚舉之前的決策點 dp(p, q)，轉移耗時 O(n^2)。不過若我們定義 dp(i, j) 表示只看 s[0~i] 與 t[0~j] 的 LCS 長度，而 i, j 不一定要配，那麼我們進而可以得到以下轉移：

- 若 s[i] == t[j]： dp(i, j) = max{ dp(i, j-1), dp(i-1, j) , dp(i-1, j-1) + 1}

- 否則：dp(i, j) = max{ dp(i, j-1), dp(i-1, j) }

答案就是 dp(n-1, m-1)。我們可以發現若一個狀態定義改變一下，或許就能更快速的轉移，實現更快的效率。

## 題目

???+note "[USACO 2016 DEC Team Building P](https://www.luogu.com.cn/problem/P2098)"
    給一個長度為 $n$ 的序列 $a$，一個長度為 $m$ 的序列 $b$，從兩個序列各選出 $k$ 個元素，滿足 $a$ 選的最大元素大於 $b$ 選的最大元素，$a$ 選的次大元素大於 $b$ 選的次大元素，以此類推，問有幾種選法。

    $1\le n,m\le 1000, 1\le k\le 10$

	??? note "思路"
        定義 dp(i, j, k) 為考慮 a 的前 i 個，b 的前 j 個，共選了 k 個的方案數。 轉移為

        - 選：if (a[i] > b[j]) dp(i, j, k) += dp(i - 1, j - 1, k - 1)
        - 不選：dp(i, j, k) += dp(p, q, k - 1)

        由於 dp(i, j, k) 需要用到所有 dp(p, q, k - 1) 的關係，我們在實作上必須先枚舉 k 這維。轉移式的部分，我們可以使用二維前綴和優化，或者是我們每次枚舉完一個 k 後，我們對於整個 dp(i, j, k) 做一次前綴加總。複雜度 O(nmk)。

        把所有牛放在一起從大到小排序，相同則 $A$ 的牛放在前面，問題變為從序列中選擇一些元素，使得任意前綴 $A$ 入選的個數大於 $B$ 入選的個數。

        $dp(i, j, k)$ 表示考慮了前 $i$ 頭牛，$A$ 入選了 $j$ 頭，$B$ 比 $A$ 少 $k$ 頭，轉移就判斷下一頭牛是 $A$ 的還是 $B$ 的，如果是 $B$ 還要判斷當前狀態 $k$ 是不是等於 $0$。複雜度 $O((n+m)k^2)$
	
	??? note "code1"
        ```cpp linenums="1"
        #include <bits/stdc++.h>
        using namespace std;

        const int mod = 1e9 + 9;
        int a[1005], b[1005], dp[1005][1005][12];

        int main() {
            int n, m, p;
            cin >> n >> m >> p;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }
            for (int i = 1; i <= m; i++) {
                cin >> b[i];
            }
            sort(a + 1, a + n + 1);
            sort(b + 1, b + m + 1);
            for (int i = 0; i <= n; i++) {
                for (int j = 0; j <= m; j++) {
                    dp[i][j][0] = 1;
                }
            }
            for (int k = 1; k <= p; k++) {
                for (int i = 1; i <= n; i++) {
                    for (int j = 1; j <= m; j++) {
                        if (a[i] > b[j]) {
                            dp[i][j][k] = dp[i - 1][j - 1][k - 1];
                        }
                    }  
                }
                for (int i = 1; i <= n; i++) {
                    for (int j = 1; j <= m; j++) {
                        dp[i][j][k] = (dp[i][j][k] + dp[i][j - 1][k]) % mod;
                    }
                }
                for (int i = 1; i <= n; i++) {
                    for (int j = 1; j <= m; j++) {
                        dp[i][j][k] = (dp[i][j][k] + dp[i - 1][j][k]) % mod;
                    }
                }
            }
            cout << dp[n][m][p];
        }
        ```
	??? note "code2"
        ```cpp linenums="1"
        #include <bits/stdc++.h>
        using namespace std;

        const int mod = 1e9 + 9;
        int a[1005], b[1005], dp[1005][1005][12];

        int main() {
            int n, m, p;
            cin >> n >> m >> p;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }
            for (int i = 1; i <= m; i++) {
                cin >> b[i];
            }
            sort(a + 1, a + n + 1);
            sort(b + 1, b + m + 1);
            for (int i = 0; i <= n; i++) {
                for (int j = 0; j <= m; j++) {
                    dp[i][j][0] = 1;
                }
            }
            for (int k = 1; k <= p; k++) {
                for (int i = 1; i <= n; i++) {
                    for (int j = 1; j <= m; j++) {
                        dp[i][j][k] = (dp[i][j][k] + dp[i - 1][j][k]) % mod;
                        dp[i][j][k] = (dp[i][j][k] + dp[i][j - 1][k]) % mod;
                        dp[i][j][k] = (dp[i][j][k] - dp[i - 1][j - 1][k] + mod) % mod;
                        if (a[i] > b[j]) {
                            dp[i][j][k] = (dp[i][j][k] + dp[i - 1][j - 1][k - 1]) % mod;
                        }
                    }  
                }
            }
            cout << dp[n][m][p];
        }
        ```
	??? note "code3"
        ```cpp linenums="1"
        #include <bits/stdc++.h>
        using namespace std;

        struct Node {
            int x, t;

            bool operator<(const Node &rhs) const {
                if (x == rhs.x) {
                    return rhs.t < t;
                }
                return rhs.x < x;
            }
        } a[2009];

        const int mod = 1e9 + 9;
        int n, m, p, dp[2009][2009][20];

        signed main() {
            cin >> n >> m >> p;
            for (int i = 1; i <= n; i++) {
                int x;
                cin >> x;
                a[i] = {x, 1};
            }
            for (int i = 1; i <= m; i++) {
                int x;
                cin >> x;
                a[i + n] = {x, 2};
            }
            sort(a + 1, a + 1 + n + m);
            dp[0][0][0] = 1;
            for (int i = 0; i <= n + m; i++) {
                for (int j = 0; j <= min(i, p); j++) {
                    for (int k = 0; k <= j; k++) {
                        dp[i + 1][j][k] = (dp[i + 1][j][k] + dp[i][j][k]) % mod;
                        if (a[i + 1].t == 1) {
                            dp[i + 1][j + 1][k + 1] = (dp[i + 1][j + 1][k + 1] + dp[i][j][k]) % mod;
                        }
                        if (a[i + 1].t == 2 && k >= 1) {
                            dp[i + 1][j][k - 1] = (dp[i + 1][j][k - 1] + dp[i][j][k]) % mod;
                        }
                    }
                }
            }
            cout << dp[n + m][p][0] << '\n';
        }
        ```

