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

???+note "[JOI 2023 Stone Arranging 2](https://loj.ac/p/3940)"
	依序給 $n$ 個 $a_i$，當 $i$ 加入時，令 $j$ 為 index 最大且 $a_j=a_i$，若存在，則將 $a_i,\ldots ,a_j$ 都設為 $a_i$，輸出最後的 $a_1, \ldots ,a_n$ 
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		【觀察】: 從 $[j,i]$ 都會被 $i$ 支配，也就是若 $i$ 改變顏色 $[j, i]$ 也會跟著改變成同一種顏色
	    
	    所以其實我們可以只記錄這些支配別人的 $i$，類似單調 stack。每次加入新的 $i$ 的時候，pop 後面直到碰到同樣顏色為止，還要再開一個 bool 陣列紀錄前面是否有出現過同樣的顏色
	    
???+note "[CF 1886 C. Decreasing String](https://codeforces.com/contest/1886/problem/C)"
	給一個字串 $s$，每次會將 $s$ 移除一個字元，使剩下的 $s$ 字典序最小。將這個過程的 $s$ 併起來，問第 $k$ 項是多少
	
	$1\le |s| \le 10^6,s$ 為 a-z
	
	??? note "思路"
		我們先求最後一個併起來的 $s$ 的長度會是多少
	
		依照字典序的定義 : 存在一個最小的 index $i$ 滿足 $a_i<b_i$，我們現在從左往右看，若發現當前的元素都是遞增的，那麼我們將最後一個元素刪除會是最好的，因為如果刪除其他元素會使大的被往前推 ; 若發現當前元素有一個突然遞減，那我們盡量將這個元素移往開頭會是最好的，所以這個過程可用單調 stack 來維護