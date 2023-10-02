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

array shrinking

atcoder dp contest slimes

2023 toi 2, pB