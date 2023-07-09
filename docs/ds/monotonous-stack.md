## 單調 stack

???+note "面試題"
	给定一个无序数列, 找出右边第一个比他大的数
	
	$O(n)$
	
	??? note "思路"
		<https://blog.csdn.net/weixin_36888577/article/details/88724916>

## 單調 queue

???+note "[CF 372 C. Watching Fireworks is Fun](https://codeforces.com/problemset/problem/372/C)"
	有 $n$ 個位置，有 $m$ 個煙花要放，每秒鐘可以移動 $d$ 長度的距離，每個煙花放出的地點和時間分別為 $a_i$ 和 $t_i$，如果在煙花放出時你所在位置為 $x$ 那麼得到的快樂值為 $b_i-|a_i-x|$，問你能得到的最大快樂值是多少？
	
	- $n\le 1.5\times 10^5,m\le 300$
	
	- $b_i,t_i\le 10^9$
	
	- $1\le d,a_i\le n$
	
	- $t_i\le t_{i+1}$
	
	??? note "思路"
		設看第 $i$ 次煙花時所在位置為 $j$，那麼轉移方程為 
		
		$$dp[i][j]=\max\{ dp[i-1][k] + b_i-|a_i-j|\}$$
		
		其中 $k\in [ j-d\times (t_i-t_{i-1}), j+d\times (t_i-t_{i-1}) ]$。那就等價於對每一次煙花的每一個位置 $j$ 我們都要求長度為 $2\times d\times t_i$ 區間內的最大值，那麼問題又變成求滑動窗口內的最大值。注意這裡的窗口長度有左右兩部分。
	
	??? note "code"	
		```cpp linenums="1"
		#pragma GCC optimize("O3,unroll-loops")
	    #pragma GCC target("avx,popcnt,sse4,abm")
	    #include <bits/stdc++.h>
	    using namespace std;
	
	    #ifdef WAIMAI
	    #define debug(HEHE...) cout << "[" << #HEHE << "] : ", dout(HEHE)
	    void dout() {cout << '\n';}
	    template<typename T, typename...U>
	    void dout(T t, U...u) {cout << t << (sizeof...(u) ? ", " : ""), dout(u...);}
	    #else
	    #define debug(...) 7122
	    #endif
	
	    //#define int long long
	    #define ll long long
	    #define Waimai ios::sync_with_stdio(false), cin.tie(0)
	    #define FOR(x,a,b) for (int x = a, I = b; x <= I; x++)
	    #define pb emplace_back
	    #define F first
	    #define S second
	
	    const int SIZE = 15e4 + 5;
	
	    int n, m, d;
	    tuple<int, int, int> tp[SIZE];
	    ll dp[SIZE], tmp[SIZE];
	
	    void solve() {
	        cin >> n >> m >> d;
	        FOR (i, 1, m) {
	            auto &[t, a, b] = tp[i];
	            cin >> a >> b >> t;
	        }
	        sort(tp + 1, tp + m + 1);
	        int last = 0;
	        FOR (i, 1, m) {
	            FOR (j, 1, n) tmp[j] = dp[j], dp[j] = 0;
	            auto [t, a, b] = tp[i];
	
	            int rp = 0;
	            deque<pair<int, ll>> st;
	            FOR (j, 1, n) {
	                int L = max(1ll, j - 1ll * (t - last) * d), R = min((ll) n, j + 1ll * (t - last) * d);
	                while (rp < R) {
	                    rp++;
	                    while (st.size() && tmp[rp] >= st.back().S) st.pop_back();
	                    st.pb(rp, tmp[rp]);
	                }
	                while (st.front().F < L) st.pop_front();
	                dp[j] = st.front().S;
	            }
	            FOR (j, 1, n) dp[j] += b - abs(a - j);
	            last = t;
	        }
	        cout << *max_element(dp + 1, dp + n + 1) << '\n';
	    }
	
	    int32_t main() {
	        Waimai;
	        int tt = 1;
	        //cin >> tt;
	        while (tt--) solve();
	    }
		```