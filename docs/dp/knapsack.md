1. https://tioj.ck.tp.edu.tw/contests/70/problems/1508

???+note "[CF 19 B. Checkout Assistant](https://codeforces.com/contest/19/problem/B)"
	有 $n$ 件物品，每件物品有價格 $c_i$ 和收銀員掃描時間 $t_i$，當收銀員掃描物品時，可以偷物品，偷一件物品只需一秒，求最少需要花費多少錢（物品順序可以隨意決定）
	
	$n\le 2000,\le 0\le t_i\le 2000,1\le c_i\le 10^9$
	
	??? note "思路"
		掃描一件物品需要 $t_i$ 時間，言外之意就是在此期間，我們可以偷走 $t_i$ 件物品，也就是對於第 $i$ 件物品，我們可以得到 $t_i+1$ 件物品。問題就轉化為 : 
		
		給 $n$ 件物品，第 $i$ 件物品重量是 $t_i+1$，花費是 $c_i$，求**至少**得到 $n$ 個重量所需的最小花費。
		
		$dp[i][j]=$ 考慮前 $i$ 個物品，得到重量總合為 $j$ 最小花費
		
		$dp[i][j]=\min \begin{cases} dp[i - 1][j] \\ dp[i - 1][j - (t_i + 1)] + c_i\end{cases}$
		
		重量總和最大有可能到 $2n$（前面的 $\sum (t_i+1)=n-1$，最後來了一個 $t_i+1=n+1$ 的），所以複雜度 $O(n\times 2n)$
		
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

        const int INF = 2e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        struct Item {
            int t, c;
        };

        int n, W;
        vector<Item> items; 

        void init() {
            cin >> n;
            for (int i = 0; i < n; i++) {
                int t, c;
                cin >> t >> c;
                W = max(W, t);
                items.pb({t, c});
            }
            W += n;
        }

        void solve() {
            vector<int> dp(W + 1, INF);
            dp[0] = 0;
            for (int i = 0; i < n; i++) {
                int t = items[i].t, c = items[i].c;
                for (int j = W; j >= (t + 1); j--) {
                    dp[j] = min(dp[j], dp[j - (t + 1)] + c);
                }
            }
            int mn = INF;
            for (int j = n; j <= W; j++) {
                mn = min(mn, dp[j]);
            }
            cout << mn << '\n';
        }

        signed main() {
            init();
            solve();
        } 
		```