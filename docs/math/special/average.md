- [貪心 - APCSC](/wiki/math/special/images/1.pdf)
- https://atcoder.jp/contests/abc236/tasks/abc236_e

### 平均最大化

???+note "[P1570 KC 喝咖啡](P1570 KC 喝咖啡)"
	$N$ 個物品重量與價值分別為 $w_i$, $v_i$，選 $K$ 個，求單位重量的價值最大多少
	
	??? note "思路"
		$$\begin{align}& \dfrac{\sum v_i}{\sum w_i}\ge x \\ \\ \Rightarrow & \sum v_i \ge \sum(x\times w_i) \\ \\  \Rightarrow & \sum (v_i - x\times w_i) \ge 0 \end{align}$$
		
		二分搜 $x$，選 $(v_i - x\times w_i)$ 最大的 $K$ 個判斷
		
	??? note "code"
		```cpp linenums="1"
		bool check (double x) {
	        for (int i = 0; i < n; i++) {
	            c[i] = w[i]- x * v[i];
	        }
	        sort(c, c + n,greater<double>());
	        
	        float sum = 0;
	        for (int i = 0; i < k; i++) {
	            sum += c[i];
	        }
	        
	        return sum >= 0;
	    }
	
	    while (R - L > 0.0001) {
	        double m = (L + R) / 2;
	        if (check (m)) {
	            L = m;
	        } else {
	            R = m;
	        }
	    }
	    ```

### 區間平均
???+note "區間平均"
	給定 $a_1,...,a_n$ 問可否選出 $a_l,..., a_r$ 使得平均為 $x$
    
    ??? note "思路"
        將 $a_i$  的變成 $a_i - x$
        
        問題就變成取 $pre_i- pre_j = 0$


### 最大平均區間
???+note "[CF EDU A. Maximum Average Segment](https://codeforces.com/edu/course/2/lesson/6/4/practice/contest/285069/problem/A)"
    給定 $a_1,...,a_n$ 問可否選出長度 $\ge k$ 的 subarray 使得平均最大，輸出這個 subarray 的左右界
    
    - $k\le n \le 10^5,0\le a_i\le 100$
    
    ??? note "思路"
    	二分搜平均值 $x$
    	
    	$\texttt{check} (x)$ 是檢查有沒有平均大於等於 $x$ 的
    	
    	也就是看 $pre(i) - pre(j) \ge 0$
    	
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
    
        int n, k;
        int ansl, ansr;
        int a[maxn];
        double b[maxn];
    
        bool check(double x) {
            for (int i = 1; i <= n; i++) {
                b[i] = (double)b[i - 1] + a[i] - x;
            }
            double mn = 0;
            int idx = 1;
            for (int i = k; i <= n; i++) {
                if (b[i - k] < mn) { // 技巧
                    mn = b[i - k];
                    idx = i - k + 1;
                }
                if (b[i] - mn >= 0) {
                    ansl = idx, ansr = i;
                    return true;
                }
            }
            return false;
        }
    
        signed main() {
            cin >> n >> k;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }
    
            double l = 0, r = 105;
            for (int i = 0; i < 100; i++) {
                double mid = (double)(l + r) / 2;
                if (check(mid)) l = mid;
                else r = mid;
            }
            check(l);
            cout << ansl << ' ' << ansr << '\n';
        } 
        ```

### 最短平均路

???+note "最短平均路 [CF EDU B. Minimum Average Path](https://codeforces.com/edu/course/2/lesson/6/4/practice/contest/285069/problem/B)"
	給定一個 $n$ 點 $m$ 邊的 DAG，邊帶權，問從 $1\to n$ 的 path 上權重的平均最小可以是多少
	
	$n\le 10^5,m\le 10^5$
	
	??? note "思路"
		跟上面最大平均區間一樣，我們去二分搜平均值 $x$，將邊權都 $-x$，看有沒有一條 path 的總和 $\le 0$。我們可以令 $dp[i]=$ 走到點 $i$ 的最小權值是多少，$dp[v]=\min \{dp[u]+w \}$ 
		
	??? note "code"
		```cpp 
		#include <bits/stdc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;

        const double INF = 2e18;
        const int maxn = 3e5 + 5;
        const int M = 1e9 + 7;

        int n, m;
        double dp[maxn];
        int par[maxn];
        vector<pii> G[maxn];

        bool check(double x) {
            fill(dp, dp + n + 1, INF);
            fill(par, par + n + 1, -1);
            dp[1] = 0;
            for (int i = 1; i <= n; i++) {
                for (auto [v, w] : G[i]) {
                    double val = (double)dp[i] + w - x;
                    if (dp[v] > val) {
                        dp[v] = val;
                        par[v] = i;
                    }
                }
            }
            return dp[n] <= 0;
        }

        signed main() {
            cin >> n >> m;
            for (int i = 1; i <= m; i++) {
                int u, v, w;
                cin >> u >> v >> w;
                G[u].pb({v, w});
            }

            double l = 0, r = 105;
            for (int i = 0; i < 100; i++) {
                double mid = (l + r) / 2;
                if (check(mid)) r = mid;
                else l = mid;
            }
            check(l);

            stack<int> stk;
            int x = n;
            stk.push(x);
            while (par[x] != -1) {
                x = par[x];
                stk.push(x);
            }
            cout << stk.size() - 1 << '\n';
            while (stk.size()) {
                cout << stk.top() << ' ';
                stk.pop();
            }
        } 
        ```