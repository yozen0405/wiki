???+note "[2023 TOI 4 模 p2. 整除序列 (Divide)](https://drive.google.com/file/d/1CvB1-JyGmFaKq1Ir8VJ5avx7NUFT2Hl9/view)"
	有一個長度為 $n$ 的陣列 $a_1,\ldots, a_n$，你只知道每個數字的其中一個 character，要構造出陣列，使得相鄰的兩個數字互為因數或倍數關係，且補上的 character 數量要越少越好
	
	$2\le n\le 10^5$
	
	??? note "思路"
		可以觀察到每一項的數字都不會太大，所以可以先猜一個 threshold T
		
		然後去 dp(i, x) : 0~i 要讓 a[i] = x 最少要填的 character 數量
		
		f(i, k) : 0~i 要讓 a[i]|k 或 k|a[i] 最少要填的 character 數量
		
		f(i, k) = min{dp(i, x) + size(x) - 1} 其中 x|k or k|x 且 dp(i, x) 有解
		
		dp(i + 1, k) = 
		
		- k 有 b[i + 1]: f(i, k) + size(k) - 1
	
		- otherwise: -1
	
		可以先預處理 0~T 每個數字會不會出現某個 character exist[x][0~9]。這樣對於每個 i 都要做一次篩法從 dp(i, x) 轉移到 f(i, k)，複雜度 O(n * T * log(T))。

???+note "[CF 1860 D. Balanced String](https://codeforces.com/contest/1860/problem/D)"
	給你一個由 01 組成的字串 s，在一次操作下，你可以 swap 任意兩項。問最少幾次操作可讓 s 的逆序數對數量（10） = 正序數對數量（01）
	
	$3\le |s| \le 100$
	
	??? note "思路"
		先想考慮將這個字串 ramdom shuffle 後，怎麼計算最少要 swap 幾次 ? 其實只要看 a[i] != b[i] 的數量除以 2 即可（因為每次 swap 一定可以換好兩個）
	
		dp[i][j][k] : 代表到第 i 個位置為止共放了 j 個 1，10 數量為 k，跟原本的 a[i] != b[i] 的數量最少有幾個
		
		dp[i][j][k] 轉移到 :
	    
	    - 放 0 : dp[i+1][j+1][k + j]
	
	    - 放 1 : dp[i+1][j][k]
		
		這樣可能會 MLE，考慮到 dp[i + 1][ ][ ] 只用到 dp[i][ ][ ]，所以可以滾動。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	    #define ll long long
	    #define pb push_back
	    #define all(x) (x).begin(), (x).end()
	    #define pii pair<int, int>
	    const int mod = 998244353, N = 105;
	
	    int dp[2][N][N * N];
	
	    void solve() {
	        string s; cin >> s;
	        int n = s.length();
	        int tot = 0, cnt0 = 0, cnt1 = 0;
	        for (char c : s) {
	            (c == '0' ? cnt0 : cnt1)++;
	        }
	        tot = n * (n - 1) / 2;
	        tot -= cnt0 * (cnt0 - 1) / 2;
	        tot -= cnt1 * (cnt1 - 1) / 2;
	        assert(tot % 2 == 0);
	
	        for (int i = 0; i < 2; ++i) {
	            for (int j = 0; j < N; ++j) {
	                for (int k = 0; k < N * N; ++k) {
	                    dp[i][j][k] = 1 << 30;
	                }
	            }
	        }
	
	        int now = 0;
	        dp[1][0][0] = 0;
	        for (int i = 0; i < n; ++i) {
	            for (int j = 0; j < N; ++j) {
	                for (int k = 0; k < N * N; ++k) {
	                    dp[now][j][k] = 1 << 30;
	                }
	            }
	            for (int j = 0; j <= n; ++j) {
	                for (int k = 0; k <= n * (n - 1) / 2; ++k) {
	                    if (dp[now ^ 1][j][k] <= n) {
	                        int c0 = (s[i] == '1'), c1 = c0 ^ 1;
	                        // 0
	                        dp[now][j][k + j] = min(dp[now][j][k + j], dp[now ^ 1][j][k] + c0);
	                        // 1
	                        dp[now][j + 1][k] = min(dp[now][j + 1][k], dp[now ^ 1][j][k] + c1);
	                    }
	                }
	            }
	            now ^= 1;
	        }
	        cout << dp[now ^ 1][cnt1][tot / 2] / 2 << '\n';
	    }
	
	    int main() {
	        ios::sync_with_stdio(false), cin.tie(0);
	        int t = 1;
	        // cin >> t;
	        while (t--) {
	            solve();
	        }
	    }
	    ```
	    
???+note "[CF 1861 E. Non-Intersecting Subpermutations](https://codeforces.com/contest/1861/problem/E)"
	給 $n,k$，對於一個長度為 $n$，每個元素皆在 $1\ldots k$ 的 array，cost 的計算方式為「最多可切出幾段 $1\ldots k$ 的 permutation」，輸出所有可能的 array 的 cost 總合為多少
	
	$2\le k\le n\le 4000$
	
	??? note "思路"
		想法 : 對於一個 subarray，貢獻了幾個 array

        dp[i] = 有幾個長度為 i 的 array 滿足 subarray(i - k + 1, i) 是有貢獻到的

        [i - k + 1, i] 一定要是一個 1~k 的 permutation，而之前的每一格我們擺 1~k 都可以，所以 initially dp[i] = k! * exp(k, i - k)

        考慮要從 dp[i] 中扣掉不合法的 case，也就是有幾個 permutation 的結尾在 [i - k + 1, i) 就已形成。設 j $\in$ [i- k + 1, i)，我們需要將 dp[i] -= dp[j] * (i - j)!

        最後，對於每個 i，因為後面可以隨便擺，對答案的貢獻就是 dp[i] * exp(k, n - i)
        
        > 參考自 : <https://codeforces.com/blog/entry/119891?#comment-1064151>
    
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        const int N = 4005;
        const int M = 998244353;

        int n, k;
        int dp[N], fac[N], powK[N];

        void init() {
            powK[0] = 1;
            fac[0] = 1;
            for (int i = 1; i <= n; i++) {
                powK[i] = (powK[i - 1] * k) % M;
                fac[i] = (fac[i - 1] * i) % M;
            }
        }

        signed main() {
            cin >> n >> k;
            init();

            for (int i = k; i <= n; i++) {
                dp[i] = (fac[k] * powK[i - k]) % M;
                for (int j = i - k + 1; j < i; j++) {
                    dp[i] = (dp[i] - (dp[j] * fac[i - j]) % M + M) % M;
                }
            }

            int ans = 0;
            for (int i = k; i <= n; i++) {
                ans = (ans + dp[i] * powK[n - i]) % M;
            }
            cout << ans << '\n';
        }
        ```