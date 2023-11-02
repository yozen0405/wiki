## 01 背包

???+note "[Atcoder DP Contest - Knapsack 1](https://atcoder.jp/contests/dp/tasks/dp_d)"
	給 $n$ 種物品的重量 $w_i$ 與價值 $v_i$，背包容量上限是 $W$。每種物品只有一個。選擇一些物品，使得重量總和不超過容量，請問最大價值總和為何 ?
	
	$n\le 100, 1\le w_i \le W\le 10^5, 1\le v_i \le 10^9$

$dp(i, j)$ 表示前 $i$ 個物品中，重量總和為 $j$ 的最大價值。轉移的話考慮拿不拿第 $i$ 種物品

$$
dp(i, j) = \max \{dp(i - 1, j - w_i) + v_i, dp(i-1, j) \}
$$

初始狀態則將 $dp(0, 0) = 0, dp(0, j) = -\infty$。最後，答案就是 $\max\{ dp(n, 0), dp(n, 1), \ldots , dp(n, W) \}$

??? note "code"
	```cpp linenums="1"
	#include <bits/stdc++.h>

    using namespace std;
    
    const int MAXN = 105;
    const int MAXW = 1e5 + 5;
    const int INF = 1e9;
    int v[MAXN], w[MAXN];
    int dp[MAXN][MAXW];
    
    int main() {
        int n, W;
        cin >> n >> W;
        for (int i = 1; i <= n; i++) {
            cin >> v[i] >> w[i];
        }
        dp[0][0] = 0;
        for (int j = 1; j <= W; j++) {
            dp[0][j] = -INF;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= W; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= w[i]) {
                    dp[i][j] = max(dp[i][j], dp[i - 1][j - w[i]] + v[i]);
                }
            }
        }
        int ans = 0;
        for (int j = 0; j <= W; j++) {
            ans = max(ans, dp[n][j]);
        }
        cout << ans << endl;
    }
    ```

??? note "code(滾動陣列)"
	```cpp linenums="1"
	#include <bits/stdc++.h>

    using namespace std;
    
    const int MAXN = 105;
    const int MAXW = 1e5 + 5;
    const int INF = 1e9;
    int v[MAXN], w[MAXN];
    int dp[MAXN];
    
    int main() {
        int n, W;
        cin >> n >> W;
        for (int i = 1; i <= n; i++) {
            cin >> w[i] >> v[i];
        }
        dp[0] = 0;
        for (int j = 1; j <= W; j++) {
            dp[j] = -INF;
        }
        for (int i = 1; i <= n; i++) {
            for (int j = W; j >= w[i]; j--) {
                dp[j] = max(dp[j], dp[j - w[i]] + v[i]);
            }
        }
        int ans = 0;
        for (int j = 0; j <= W; j++) {
            ans = max(ans, dp[j]);
        }
        cout << ans << endl;
    }
    ```

### 方法數

???+note "算方法數 [CSES - Two Sets II](https://cses.fi/problemset/task/1093/)"
	給 $n$，問將 $\{1, 2, \ldots ,n \}$ 分成兩個總和相同的集合，有幾種分法
	
	$n\le 500$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    using namespace std;
	
	    const int INF = 0x3f3f3f3f;
	    const int M = 1e9 + 7;
	
	    signed main() {
	        int n;
	        cin >> n;
	        vector<int> w(n + 1);
	        vector<int> v(n + 1);
	        int W = 0;
	        for (int i = 1; i <= n; i++) {
	            W += i;
	            w[i] = i;
	            v[i] = i;
	        }
	        if (W & 1) {
	            cout << 0;
	            exit(0);
	        }
	        W >>= 1;
	        vector<int> cnt(W + 1);
	        vector<int> dp(W + 1, -INF);
	        dp[0] = 0;
	        cnt[0] = 1;
	        for (int i = 1; i <= n; i++) {
	            for (int j = W; j >= w[i]; j--) {
	                if (dp[j] == dp[j - w[i]] + v[i]) {
	                    cnt[j] += cnt[j - w[i]];
	                    cnt[j] %= M;
	                } else if (dp[j - w[i]] + v[i] > dp[j]) {
	                    dp[j] = dp[j - w[i]] + v[i];
	                    cnt[j] = cnt[j - w[i]];
	                    cnt[j] %= M;
	                }
	            }
	        }
	        int mx = -INF;
	        cnt[W] *= 500000004;
	        cnt[W] %= M;
	        cout << cnt[W];
	    }
	    ```

### 變化

???+note "[超大背包 Atcoder DP Contest - Knapsack 2](https://atcoder.jp/contests/dp/tasks/dp_e)"
	給 $n$ 種物品的重量 $w_i$ 與價值 $v_i$，背包容量上限是 $W$。每種物品只有一個。選擇一些物品，使得重量總和不超過容量，請問最大價值總和為何 ?
	
	$n\le 100, 1\le w_i \le W\le 10^9, 1\le v_i \le 10^3$

???+note "[TOI 2008 p3. 加減問題](https://tioj.ck.tp.edu.tw/contests/70/problems/1508)"
	有 $n$ 個正整數 $a_1, \ldots ,a_n$，對於每個數賦予 $\texttt{+}$ 或 $\texttt{-}$ 使得他們總合為 $0$，問是否做得到
	
	有 $t$ 筆輸入，$t\le 10, n\le 100, 1\le a_i\le 1000$

## 無限背包

???+note "[洛谷 P1616 疯狂的采药](https://atcoder.jp/contests/dp/tasks/dp_d)"
	給 $n$ 種物品的重量 $w_i$ 與價值 $v_i$，背包容量上限是 $W$。每種物品只有一個。選擇一些物品，使得重量總和不超過容量，請問最大價值總和為何 ?
	
	$n\le 100, 1\le w_i \le W\le 10^5, 1\le v_i \le 10^9$

跟 01 背包差不多，但要注意在轉移的時候，對於 $dp(i, j)$ 取完物品 $i$ 後狀態依然會停留在 $dp(i,j-w_i)$ 而非 $dp(i-1,j-w_i)$

$$
dp(i, j) = \max \{dp(i, j - w_i) + v_i, dp(i-1, j) \}
$$

最後，答案就是 $\max\{ dp(n, 0), dp(n, 1), \ldots , dp(n, W) \}$

## 有限背包

???+note "[CSES - Book Shop II](https://cses.fi/problemset/task/1159/)"
	給 $n$ 種物品的重量 $w_i$，價值 $v_i$，數量 $c_i$，背包容量上限是 $W$。選擇一些物品，使得重量總和不超過容量，請問最大價值總和為何 ?
	
	$n\le 100, 1\le W \le 10^5, 1\le w_i,v_i,c_i \le 1000$

每個物品的 $c_i$ 個都當成一個物品，套用 01 背包。複雜度 $O(nW\max \{ c_i \})$

### 二進制拆解

把 c 個一樣的物品拆成 log c 個不同的物品，執行 0/1 背包。例如有一種東西有 13 個，13 = 1 + 2 + 4 + 6，我們就按照 1 個、2 個、4 個、6 個把東西打包，變成 01 背包，這樣我們就可以組出在 [1, 13] 內任意的數字了。複雜度 $O(nW\log  c_i)$

??? info "正確性證明"
	我們都知道，若我們有 $2^0, 2^1, 2^2, \ldots ,2^n$ 幾個數字，那我們可以表示 $[1, 2^{(n + 1)} - 1]$ 內的所有數字。相似的，若我們想要湊出一個 $c$ 以內所有的數字，我們可以將 $c$ 拆解為 $k$ 與 $2^n - 1$ 兩個部分，並使 $n$ 盡量大，接著 $2^n - 1$ 便可以拆解為 $2^0, 2^1, 2^2, \ldots ,2^n$。如此一來我們就可以湊出 $[1, 2^{(n + 1)} - 1]$ 的所有數字以及 $[1+k, 2^{(n + 1) - 1}+k]$ 的所有數字，聯集起來，就是可以表示 $[1, c]$ 的所有數字。例如 $c=10$，$10=(1+2+4)+3$，代表我們可以湊出 $[1, 7] \cup [3, 10]$ 的所有數字。若需背包一種數量有 $c$ 個的物品，就可以依照上法將之拆為 $\log(c) + 1$ 團，我們只需要將每團各自視為一個新的物體拿去背包即可。
	
	> 相關議題: [砝碼問題](https://blog.csdn.net/songchuwang1868/article/details/86535150)

??? note "code"
	```cpp linenums="1"
	vector<int> vec;
    for (int i = 0; i < (int)w.size(); i++) {
        int k = 1;
        while (k < cnt[i]) {
            vec.push_back(w[i] * k);
            cnt[i] -= k;
            k *= 2;
        }
        if (k > 0) {
            vec.push_back(w[i] * k);
        }
        /*
        slow version
        for (int j = 0; j < cnt[i]; j++) {
            vec.push_back(w[i]);
        }
        */
    }

    solve(vec);  // 01 背包
	```

## 單調對列優化

??? note "code"
	```cpp linenums="1"
	for (int i = 1; i <= n; i++) {
        for (int j = 0; j < w[i]; j++) dq[i].clear();
        for (int j = 0; j <= s; j++) dp[i - 1][j] -= v[i] * 1LL * (j / w[i]);
        for (int j = 0; j <= s; j++) {
            int idx = j % w[i];
            if (!dq[idx].empty() && dq[idx].front() < j - w[i] * 1 LL * c[i])
                dq[idx].pop_front();
            dp[i][j] = dp[i - 1][j];
            if (j >= w[i]) dp[i][j] = max(dp[i][j], dp[i - 1][dq[idx].front()] + j / w[i] * 1LL * v[i]);

            while (!dq[idx].empty() && dp[i - 1][dq[idx].back()] <= dp[i - 1][j])
                dq[idx].pop_back();
            dq[idx].push_back(j);
        }
    }
    ```

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

??? info "貪心法錯誤"
	$n=3,W=6$

    - $v_1=4,w_1=5$
    
    - $v_2=3,w_2=3$
    
    - $v_3=3,w_3=3$
    
    按照 Greedy，就只能放入第 1 個物品，總價值為 $5$，但是最優解，應該放入第 2, 3 個物品，總價值為 $6$
    
    之所以貪心法會失效，原因在於背包的體積，優先選 CP 值高的物品，可能會導致一些空間被浪費，以前面的例子來說，就是浪費 2 單位的空間
    
    反過來說，下面的物品就可以使用貪心法:
    
    物品可以切割
    
    $n\le 100, m\le 10^5, v_i\le 10^5,w_i\le 10^7$
    
    物品可以切割，換句話說就是可以取 0.5 個、0.7 個物品，這樣一來，就沒有浪費空間的問題，而可以使用攤新法了
    
    $n=3,W=6$
    
    - $v_1=4,w_1=5$
    
    - $v_2=3,w_2=3$
    
    - $v_3=3,w_3=3$
    
    取 1 個第一件物品，2/3 個第二件物品，總價值為 7

---

- APCSC

- <https://hackmd.io/@Ccucumber12/Bk6lLyuxF#/>

- <https://sprout.tw/algo2023/ppt_pdf/week09/dp2_inclass_tp.pdf>

- <http://pisces.ck.tp.edu.tw/~peng/index.php?action=showfile&file=fcaa846ddcba22c1c63777723152ba9492a9f2218>

- <https://blog.csdn.net/windfriendc/article/details/123892024>

- <https://hackmd.io/-EB3-tLaSbOcNb9yn1XCyg>