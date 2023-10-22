???+note "[CF 1025 D. Recovering BST](https://codeforces.com/problemset/problem/1025/D)"
	給一個長度為 $n$ 的中序陣列 $a_1,\ldots ,a_n$，問是否能組出一顆 BST 滿足相鄰兩點的 $\gcd (a_i, a_j)=1$
	
	$2\le n\le 700, 2\le a_i \le 10^9$
	
	??? note "思路"
		令 L(i, j) 為以 j 為根，[i, j - 1] 當左子樹是否能組出一顆合法 BST
		
		令 R(i, j) 為以 i 為根，[i + 1, j] 當右子樹是否能組出一顆合法 BST
		
		對於一個區間 [l, r]，枚舉 k 當 [l, r] 的根（用 L(l, k) && R(k, r) 判斷），看看是 k 否能接到 l - 1 的右子樹或 r + 1 的左子樹，可行則分別設 R(l - 1, k) = true，或 L(k, r + 1) = true
		
		可以先預處理 G(i, j) 代表 i 是否能與 j 連邊，在轉移時維護一個 ans(l, r) 代表 [l, r] 是否能組出一顆合法的 BST，最後輸出 ans(1, n) 即可

???+note "[全國賽模擬賽 pF. 鬧鐘設置 (F_Alarm_Clock)](https://drive.google.com/file/d/1KAaPOYdzBuoC0hXdFwS5W0ZEItY61ktk/view)"
	給一個長度 $n$ 的陣列 $a_1, \ldots ,a_n$，你可以在每個位置 $k+1\le i\le n - k$ 都設置任意個鬧鐘，並決定每個鬧鐘什麼時候要響，每個鬧鐘的影響範圍是 $[i-k, i+k]$，某一個位置 $j$ 的起床時間是影響到他的所有鬧鐘中，最早響的時間，而這個時間必須 $\le a_j$。求最大的起床時間總和。
	
	$1\le n\le 500 , 2k+1\le n, 1\le a_i\le 10^6$
	
	??? note "思路"
		$dp(l,r)=[l,r]$ 範圍內的答案，轉移就是找到區間裡最小，那個後枚舉 $[i-k, i+k]$ 滿足有覆蓋到最小的（鬧鐘可以設置在 $[l, r]$ 之外），且沒有設置在最前面與最後面不合法的區段，然後 $[i-k, i+k]$ 以外的兩個區間就是子問題。
		
		<figure markdown>
	      ![Image title](./images/24.png){ width="400" }
	    </figure>

???+note "[JOI 2015 Final 分蛋糕 2](https://loj.ac/p/2725)"
	有一個蛋糕，切成 $n$ 塊，第 $i$ 塊的大小是 $a_i$。首先，Alice 先拿一塊走，接著從 Bob 開始交替選，只能選擇左右至少有一塊已被選擇的蛋糕，當有多個選擇，Bob 會選比較大的，Alice 可以任意選。問 Alice 選的蛋糕大小總和最大是多少
	
	$1\le n\le 2000, 1\le a_i \le 10^9$
	
	??? note "思路"
		dp(l, r) = 取完 [l, r]，Alice 的分數 - Bob 的分數的 max
		
		然後從長度小到長度大轉移即可，注意是環，所以我們自然會把長度弄成 2n，但 $dp(1, *)$ 也是要考慮往左轉移的 case，我的處理方法是將 a[0] = a[n]，然後最後的答案不計入 $dp(0, *)$ 即可
		
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
	
	    const int INF = 4e18;
	    const int maxn = 4e3 + 10;
	
	    int n;
	    int sum;
	    int a[maxn], dp[maxn][maxn];
	
	    signed main() {
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i];
	            sum += a[i];
	        }
	        for (int i = n + 1; i <= 2 * n; i++) {
	            a[i] = a[i - n];
	        }
	        for (int l = 0; l < maxn; l++)  {
	            for (int r = 0; r < maxn; r++) {
	                dp[l][r] = -INF;
	            }
	        }
	        for (int i = 1; i <= 2 * n; i++) {
	            dp[i][i] = a[i];
	        }
	        a[0] = a[n];
	        for (int len = 1; len <= n - 1; len++) {
	            for (int l = 1; l + len - 1 < 2 * n; l++) {
	                int r = l + len - 1;
	                if (len & 1) {
	                    int mx = max(a[l - 1], a[r + 1]);
	                    if (mx == a[r + 1]) {
	                        dp[l][r + 1] = max(dp[l][r + 1], dp[l][r] - mx);
	                    } else {
	                        dp[l - 1][r] = max(dp[l - 1][r], dp[l][r] - mx);
	                    }
	                } else {
	                    dp[l - 1][r] = max(dp[l][r] + a[l - 1], dp[l - 1][r]);
	                    dp[l][r + 1] = max(dp[l][r] + a[r + 1], dp[l][r + 1]);
	                }
	            }
	        }
	        int ans = -INF;
	        // me: 9, u: 6
	        // dp: 3, sum = 15, val = (15 - 3) / 2 + 3
	        for (int i = 1; i <= n; i++) {
	            int val = (sum - dp[i][i + n - 1]) / 2 + dp[i][i + n - 1];
	            ans = max(val, ans);
	        }
	        cout << ans << "\n";
	    }
	    ```

???+note "[POI 2015 Car washes](https://loj.ac/p/2688)"
	有 $n$ 間商店，第 $i$ 間價格 $p_i$。有 $m$ 個消費者，第 $i$ 個會從 $p_{a_i},\ldots ,p_{b_i}$ 挑價格最小的，若價格 $\le c_i$ 就會買，反之不會。構造 $p$，使花錢總和最大

	$n\le 50, m\le 4000, 1\le c_i,p_i\le 5\times 10^5$
	
	??? note "思路"
		因為最佳解一定可以透過多個 $c_i$ 構成，所以我們可以先將 $c_i$ 離散化

        $dp(l,r,k)=$ 所有區間 $[l,r]$ 內的消費者當區間最小值 $\ge k$ 時的總消費最小值
        
        $$
        dp(i,j,k)=\min \limits_{i\le pos\le j} \{dp(i,pos-1,k)+dp(pos+1,j,k)+k\cdot g(pos,k) \}
        $$

        然後因為是 $\ge k$，所以也必須考慮從 $dp(i,j,k+1)$ 轉移過來的情況

        其中 $g(pos,k)$ 表示: 在當前 $[l,r]$ 內的消費者中，經過了 $pos$ 且 $c_i\le k$ 的數量。這一部分消費者的貢獻就是 $k$

        構造一組解的話，我們紀錄:

        - $k$ 要往上跳到哪裡

        - 斷點 $pos$ 的位置

        最後用遞迴回溯答案
        
        > 參考自: <https://blog.csdn.net/qq_41996523/article/details/112477860>
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        const int MAXN = 55, MAXM = 4005;
        int n, m, a[MAXM], b[MAXM], c[MAXM], lsh[MAXM], tot;
        int g[MAXN][MAXM], f[MAXN][MAXN][MAXM], pos[MAXN][MAXN][MAXM];
        int lstk[MAXN][MAXN][MAXM], ans[MAXN];

        void getans(int l, int r, int k) {
            if (k > tot) {
                return;
            }
            if (l > r) {
                return;
            }
            k = lstk[l][r][k];
            ans[pos[l][r][k]] = lsh[k];
            getans(l, pos[l][r][k] - 1, k);
            getans(pos[l][r][k] + 1, r, k);
        }

        signed main() {
            cin >> n >> m;
            for (int i = 1; i <= m; i++) {
                cin >> a[i] >> b[i] >> c[i];
                lsh[++tot] = c[i];
            }

            sort(lsh + 1, lsh + 1 + tot);
            tot = unique(lsh + 1, lsh + 1 + tot) - lsh - 1;
            for (int i = 1; i <= m; i++) {
                c[i] = lower_bound(lsh + 1, lsh + 1 + tot, c[i]) - lsh;
            }

            for (int len = 1; len <= n; len++) {
                for (int l = 1; l + len - 1 <= n; l++) {
                    int r = l + len - 1;
                    for (int i = 1; i <= n; i++) {
                        for (int j = 1; j <= tot; j++) {
                            g[i][j] = 0;
                        }
                    }
                    for (int i = 1; i <= m; i++) {
                        if (l <= a[i] && b[i] <= r) {
                            g[a[i]][c[i]]++;
                            g[b[i] + 1][c[i]]--;
                        }
                    }
                    for (int i = 1; i <= n; i++) {
                        for (int j = 1; j <= tot; j++) {
                            g[i][j] += g[i - 1][j];
                        }
                    }
                    for (int i = 1; i <= n; i++) {
                        for (int j = tot; j >= 1; j--) {
                            g[i][j] += g[i][j + 1];
                        }
                    }
                    for (int k = tot; k >= 1; k--) {
                        for (int p = l; p <= r; p++) {
                            int val = (p > l ? f[l][p - 1][k] : 0) + (p < r ? f[p + 1][r][k] : 0) + lsh[k] * g[p][k];
                            if (f[l][r][k] <= val) {
                                f[l][r][k] = val;
                                pos[l][r][k] = p;
                            }
                        }
                        if (f[l][r][k] >= f[l][r][k + 1]) {
                            lstk[l][r][k] = k;
                        } else {
                            f[l][r][k] = f[l][r][k + 1];
                            lstk[l][r][k] = lstk[l][r][k + 1];
                        }
                    }
                }
            }
            cout << f[1][n][1] << '\n';
            getans(1, n, 1);
            for (int i = 1; i <= n; i++) {
                cout << ans[i] << ' ';
            }
        }
    	```

???+note "[CF 1312 E. Array Shrinking](https://codeforces.com/problemset/problem/1312/E) "
	給一個長度為 $n$ 的序列 $a_1, \ldots ,a_n$，相鄰的兩個 $a_i$ 若相等可以合併成一個元素 $a_i+1$，問最後最少可以剩下多少個元素。

	$n\le 500, 1\le a_i\le 1000$

	??? note "思路"
		這裡需要用到一個結論: 對於一個區間，如果它可以被合成一個數，那麼它被合成的數是唯一的。

        > 證明:
        >
        > 若將一個數 $a_i$ 定義成 $2^{a_i}$，那麼不管怎麼合併總和都是一樣的

        令 $dp(l,r)$ 為 $a_l,\ldots ,a_r$ 最後最多可以剩多少元素，$f(l,r)$ 為 $a_l,\ldots ,a_r$ 若可以最後會合併成什麼數字。轉移式
        
        $$
        dp(l,r)=\min\{dp(l,k)+dp(k+1,r) \}
        $$
        
        若 $dp(l,k)=1$ 且 $dp(k+1,r)=1$ 且 $a_{l,k}=a_{k+1,r}=1$，才可讓 $dp(l,r)=1,a_{l,r}=a_{l,k}+1$
        
???+note "[2023 TOI 二模 pB. 圓周拉弦（chord）](https://drive.google.com/file/d/15bXJ8w5d_W4iY9dGOi7gotmnUURG8Rm8/view)"
	有一個圓，上面有 $n$ 個等分點，連起兩個點 $i,j$ 所得到的 cost 是 $w_i + w_j \pmod k$，在連線只能交在端點的情況下，求最大分數。

	$3\le n\le 500,2\le k\le 500,0\le w_i\le k$
	
	??? note "思路"
		$dp(l,r)=$ 只能在 $l,\ldots ,r$ 之間連線的最大 cost

        轉移式則枚舉中間的斷點，兩邊變成子問題，記得要加上 $l$ 與 $r$ 連接的 cost
        
        $$
        dp(l,r)=\max \limits_{l<k<r}\{dp(l,k)+dp(k,r) \}+(w_l+w_r)\% k
        $$
        
        <figure markdown>
          ![Image title](./images/30.png){ width="300" }
        </figure>
        
        最後，答案就是枚舉起點與斷點 $s,t$，答案就是 $\max \{dp(s,t)+dp(t,s)-(w_s+w_t)\% k \}$
        
        <figure markdown>
          ![Image title](./images/31.png){ width="300" }
        </figure>
		
atcoder dp contest slimes
