狀態壓縮 dp，英文稱 bitmask dp，使用二進位來表示狀態，來達到類似暴力枚舉的效果

## 枚舉子集的子集

???+note "code"
	```cpp linenums="1"
	for (int mask = 0; mask < (1 << n); mask++) {
        // 給一個 mask，枚舉他的所有子集合
        for (int S = mask; S; S = (S - 1) & mask) {
            // TODO
        }
    }
    ```
    
## 題目

???+note "[Atcoder DP Contest O - Matching](https://atcoder.jp/contests/dp/tasks/dp_o)"
	有 $n$ 個男生和女生，第 $i$ 個男生與第 $j$ 的女生是否能配對取決於 $a_{i,j}=\{ 0, 1 \}$。問有幾種方法能產生 $n$ 組配對
	
	$1\le n\le 21$
	
	??? note "思路"
		可以依照 1, 2, ..., n 的順序讓男生配對，女生則用枚舉 mask
        
        dp(S) = 目前已將男生 [1, |S|] 與女生的集合 S 配對
        
        轉移 dp(S) = dp(S ^ (1 << i)) | a[|S|][i] == 1
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        const int INF = 0x3f3f3f3f;
        const int M = 1e9 + 7;
        const int maxn = 21;
        int n;
        int a[maxn][maxn];
        int dp[1 << maxn];

        signed main() {
            cin >> n;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    cin >> a[i][j];
                }
            }
            dp[0] = 1;
            for (int mask = 1; mask < (1 << n); mask++) {
                int now = __builtin_popcount(mask) - 1; 
                for (int i = 0; i < n; i++) {
                    if ((mask & (1 << i) && a[now][i])) {
                        dp[mask] += dp[mask ^ (1 << i)];
                        dp[mask] %= M;
                    }
                }
            }
            cout << dp[(1 << n) - 1];
        }
        ```

???+note "[CSES - Elevator Rides](https://cses.fi/problemset/task/1653)"
	有 $n$ 個人，第 $i$ 個人的重量為 $w_i$。給 $x$，每次可以移除一個重量和 $\le x$ 的集合，問最少移除幾次才能將 $n$ 個人都移除
	
	$n\le 20, 1\le w_i \le x \le 10^9$
	
	??? note "思路"
		如果枚舉子集的子集會需要 3^n，TLE
		
		令 dp(S) 為 S 內的人都已經移除的最小次數，f(S) 在移除次數為 dp(S) 下，最後一次移除的最小重量和為多少
		
		dp(S) = dp(S ^ (1 << i)) + (f(S ^ (1 << i)) + w[i] > x)
		
		- f(S ^ (1 << i)) + w[i] > x

			- dp(S) = dp(S ^ (1 << i)) + 1

			- f(S) = f(S ^ (1 << i)) + w[i] - x

		- f(S ^ (1 << i)) + w[i] <= x

			- dp(S) = dp(S ^ (1 << i))

			- f(S) = f(S ^ (1 << i)) + w[i]

	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        const int INF = 2e18;
        const int MAXN = 1 << 22;
        int n, x;
        int w[MAXN], dp[MAXN], t[MAXN];

        signed main() {
            cin >> n >> x;
            for (int i = 0; i < n; i++) {
                cin >> w[i];
            }

            dp[0] = 1;
            t[0] = 0;
            for (int mask = 1; mask < (1 << n); mask++) {
                dp[mask] = INF;
                t[mask] = INF;
                for (int i = 0; i < n; i++) {
                    if (mask & (1 << i)) {
                        int S = mask ^ (1 << i);
                        if (w[i] + t[S] <= x) {
                            if (dp[S] < dp[mask] || (dp[S] == dp[mask] && t[S] + w[i] < t[mask])) {
                                t[mask] = t[S] + w[i];
                                dp[mask] = dp[S];
                            }
                        } else {
                            if (dp[S] + 1 < dp[mask] || (dp[S] + 1 == dp[mask] && t[S] + w[i] < t[mask])) {
                                t[mask] = w[i];
                                dp[mask] = dp[S] + 1;
                            }
                        }
                    }
                }
            }
            cout << dp[(1 << n) - 1] << "\n";
        }
		```
	
???+note "[TIOJ 1418 . 交大都是雷](https://tioj.ck.tp.edu.tw/problems/1418)"
	有 3n 個人，給定倆倆之間若在同組的罰分。問將以 3 個為一組，分成 n 組的最少總罰分
	
	$1\le n\le 7$
	
	??? note "思路"
		每次枚舉 mask 裡面的 3 個元素 i, j, k 做轉移，而 i 其實只要枚舉一次就好，因為他無論如何都要配對，早晚不是問題
		
		注意: 需要使用 Top down，因為 Bottom up 會跑到很多沒用的狀態
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        int n, v[22][22];
        string s;
        int dp[1 << 22];

        int cost(int a, int b, int c) {
            int ret = v[a][b] + v[a][c] + v[b][c];
            return ret;
        }

        int solve(int mask) {
            if (dp[mask] > -1) return dp[mask];
            if (!mask) return 0;
            int mn = 1e9;
            for (int i = 0; i < n; i++) {
                if (!((1 << i) & mask)) continue;
                for (int j = i + 1; j < n; j++) {
                    if (!((1 << j) & mask)) continue;
                    for (int k = j + 1; k < n; k++) {
                        if (!((1 << k) & mask)) continue;
                        mn = min(mn, solve(mask ^ (1 << i) ^ (1 << j) ^ (1 << k)) + cost(i, j, k));
                    }
                }
                break;
            }
            return dp[mask] = mn;
        }

        signed main() {
            int t;
            cin >> t;
            while (t--) {
                cin >> n;
                for (int i = 0; i < n; i++) {
                    for (int j = 0; j < n; j++) {
                        cin >> v[i][j];
                    }
                }
                for (int i = 0; i < (1 << n); i++) {
                    dp[i] = -1;
                }
                cout << solve((1 << n) - 1) << "\n";
            }
        }
        ```
	
???+note "[TIOJ 1014. 打地鼠](https://tioj.ck.tp.edu.tw/problems/1014)"
	有 $n$ 個地鼠，分別在點 $1$ 到 $n$。地鼠 $i$ 每 $t_i$ 秒會出現一次，玩家從 $0$ 出發，每秒可以移動一格，問打完所有地鼠所需的最少秒數
	
	$n \leq 16$
	
	??? note "思路"
		dp(S, i) = 打完集合 S 內的地鼠，目前在位置 i
	
		dp(S, i) = dp(j, S ^ (1 << i)) + cost(j, i)
		
		cost(j, i) 實則是要將 dp(S, i) 設為 dp(j, S ^ (1 << i)) + |i - j| 下一次 t[i] 出現的時候，所以我們可以列出
		
		dp(S, i) = ceil(dp(j, S ^ (1 << i)) + |i - j|, t[i]) * t[i]
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        const int INF = (1LL << 60);
        int dp[(1 << 16)][17], t[17];

        int iceil(int a, int b){
            if(b < 0) a *= -1, b *= -1;
            if(a > 0) return (a + b - 1) / b;
            else return a / b;
        }

        signed main() {
            ios_base::sync_with_stdio(0);
            cin.tie(0);
            int n;
            cin >> n;
            for (int i = 0; i < n; i++) {
                cin >> t[i];
            }
            memset(dp, 0x3f, sizeof(dp));

            dp[0][0] = 1;
            for (int mask = 1; mask < (1 << n); mask++) {
                for (int i = 0; i < n; i++) {
                    if (mask & (1 << i) == 0) continue;
                    for (int j = 0; j < n; j++) {
                        int time = dp[mask ^ (1 << i)][j] + abs(j - i);
                        dp[mask][i] = min(dp[mask][i], iceil(time, t[i]) * t[i]);
                    }
                }
            }
            int ans = INF;
            for (int i = 0; i < n; i++) {
                ans = min(ans, dp[(1 << n) - 1][i]);
            }
            cout << ans << '\n';
        }
		```

???+note "[LeetCode 698. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)"
	給一個長度為 n 的序列 a，問能不能分成 k 組，使得每組的總和相同
	
	$1\le k\le n\le 16, 0\le a_i\le 10^4$
	
	??? note "思路"
		先看 target = a[0], ..., a[n - 1] 是否符合 target % k == 0，不是的話直接 return，是的話再將 target /= k
	
		dp(S) = S 是否能分成好幾組，每組的 sum 都是 target
		
		dp(S) = dp(S ^ U) | (sum[U] == target)
		
		複雜度 O(3^n)，其實我們在預處理 sum[U] 可以做到 O(2^n)，就是利用 
		
		<center>
		sum[U] = sum[U - lowbit(U)] + a[__builtin_ctz(lowbit(mask))]
		</center>
		
	??? note "code"
		```cpp linenums="1"
		#define lowbit(x) (x & (-x))
        class Solution {
           public:
            bool canPartitionKSubsets(vector<int>& a, int k) {
                int n = a.size();
                int tar = 0;
                for (auto ele : a) tar += ele;
                if (tar % k) return 0;
                tar /= k;
                vector<int> sum(1 << n);
                for (int mask = 1; mask < (1 << n); mask++) {
                    int idx = __builtin_ctz(lowbit(mask));
                    sum[mask] = sum[mask - lowbit(mask)] + a[idx];
                }
                vector<int> dp(1 << n);
                dp[0] = true;
                for (int mask = 1; mask < (1 << n); mask++) {
                    for (int s = mask; s > 0; s = (s - 1) & mask) {
                        if (sum[s] == tar && dp[mask ^ s]) {
                            dp[mask] = true;
                            break;
                        }
                    }
                }
                return dp[(1 << n) - 1];
            }
        };
		```

???+note "[CF 1886 E - I Wanna be the Team Leader](https://codeforces.com/contest/1886/problem/E)"
    有 $m$ 個 projects，難度分別為 $b_1, \ldots ,b_m$，有 $n$ 個人，抗壓程度分別為 $a_1, \ldots ,a_n$。現在需要分組，滿足以下條件 :

    - 使每個 project 至少有一個人做
    - 每個人至多負責一個 project
    - 令負責第 $j$ 個 project 的人有 $k$ 個，則這些 $a_i$ 須滿足 $a_i\le\frac{b_j}{k}$
    
    $n\le 2\times 10^5, m\le 20,1\le a_i, b_i\le 10^9$
    
    ??? note "思路"
        考慮將序列 $a$ 大到小 sort，每個 $b_i$ 挑的就會是一個 prefix，因為如果不是一個 prefix，那對於最後面的那格我們只在意前面選的數量，因此可將倒數第二格與前面的某個的 $b_i$ swap，還可以讓其他 $b_i$ 挑的 $a_i$ 的最小值變大，一直這樣做就會變好幾個連續區段。那問題就可以用 bitmask dp 解決，$dp[S]$ 代表 $S$ 目前 prefix 選到哪裡可使 $S$ 內的 $b_i$ 都滿足條件，轉移的話枚舉不在 $S$ 內的 $j$，看 $j$ 要延伸到哪裡，我們令 $j$ 支配的區段為  $[dp[S]+1,dp[S\cup j]]$，$dp[S\cup j]=nxt(j,dp[S])$。$nxt(j,i)$ 代表 $b_j$ 從 $i$ 開始往右選最少選到哪裡即可使 $b_j$ 滿足條件。



## Graph coloring problem

最小點著色、 k 點著色，都是 NP-complete 問題

??? info "四色定理"
	平面圖一定可以四著色，P 問題，有著 O(N²) 演算法，但不一定可以三著色。詳見 [Wiki](https://zh.wikipedia.org/zh-tw/%E5%9B%9B%E8%89%B2%E5%AE%9A%E7%90%86) 或 [演算法筆記](https://web.ntnu.edu.tw/~algo/Coloring.html)

???+note "k-Vertex Coloring"
	給一張 $n$ 點 $m$ 邊無向圖，問是否能 $k$-coloring，使每條邊兩端的顏色都不同
	
	??? note "思路"
		> k = 2
		
		二分圖判斷
		
		---
		
		> k = 3
		
		$O(m\times 2^n)$ 列舉第一種獨立集，剩下的二分圖
		
		---
		
		> k = 4
		
		令 $dp(S)=S$ 是否可以 2-colors 上色，建表複雜度 $O(m\times 2^n)$。列舉 $S$ 使得 $dp(S)$ 跟 $dp(V\setminus S)$ 都要是 true

???+note "Minimum Vertex Coloring  / Chromatic Number"
	給一張 $n$ 點 $m$ 邊無向圖，問最少塗幾種顏色可使每條邊兩端的顏色都不同
	
	??? note "思路"
		令 $dp(S)=S$ 最少多少 coloring，轉移式
		
		$$
		dp(S)=\min\limits_{T\in S} \{ dp(S \setminus T) + 1 \}
		$$
		
		其中 $T$ 要是獨立集，複雜度 $O(3^n)$
