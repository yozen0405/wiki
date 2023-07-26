- 第 k 大

- noi 2016

- 平均 

- 最大化最小值

- 雙層二分搜([Google code jam](https://www.acmicpc.net/problem/27811))

- JOI 蛋糕

- APCS double cnt overflow

- 問題 2023 toi pB (已編輯)

- APCSC binary search 提單

- [全國賽 2019 史蒂芬與獵人](https://sorahisa-rank.github.io/nhspc-fin/2019/problems.pdf#page=10)

- [neoj 田忌賽馬](https://neoj.sprout.tw/problem/69/)

- 2023 TOI mock double cnt

- <https://tioj.ck.tp.edu.tw/problems/1669>

- [JOI Kingdom](https://loj.ac/p/2334)

- 2021 nhspc pD

- whale Atcoder 

- 全國賽 2021 pG subtask 1, 2

!!! warning "check(x) 的 x 太大的時候，有些情況會造成 cnt overflow"

!!! warning "記得需要開 double 的時候，l, r, mid, check(x) 都要用 double，不能有一些是 int 有一些又是 double"

!!! warning "注意 l, r 一開始的有沒有還蓋答案的左界右界"

!!! warning "while(l < r) while(r - l > 1)"

!!! warning "TLE 有可能是二分搜壞掉導致, 可能一開始推導時有誤"

???+note "[CF 1853 C. Ntarsis' Set](https://codeforces.com/contest/1853/problem/C)"
	給一個包含 $1,2,\ldots ,10^{1000}$ 所有數字的 Set $S$，每天從 $S$ **同時**移除第 $a_1,a_2,\ldots ,a_n$ 個數字，問 $k$ 天之後 $S$ 中最小的數字是多少
	
	$n,k\le 2\times 10^5,1\le a_i \le 10^9,a_1<a_2<\ldots < a_n$
	
	??? note "hint"
		令 $a_i\le x < a_{i+1}$，過了一天之後，小於等於 $x$ 的數字就會剩下 $x-i$ 個
		
	??? note "思路"
		觀察 :
		
		- 令答案為 ans，在 ans 之前的數字一定都會被移除
	
		- 令 $a_i\le x < a_{i+1}$，當過了一天之後，小於等於 $x$ 的數字會被移除 $i$ 個，也就是小於等於 $x$ 的數字就會剩下 $x-i$ 個，然後我們再去找 $a_j\le x - i < a_{j+1}$，過了一天後，小於等於 $x$ 的數字就會剩下 $x-i-j$ 個，以此類推
	
		<figure markdown>
	      ![Image title](./images/2.png){ width="400" }
	      <figcaption>$a=[1,3,4],k=3,x=8$</figcaption>
	    </figure>
	
		二分搜最後一個在「移除區段」的數字，去 check : mid 之前的數字在經過 $k$ 天的移除後剩下幾個 
		
		<figure markdown>
	      ![Image title](./images/1.png){ width="400" }
	    </figure>
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	
	    using namespace std;
	
	    typedef long long ll;
	    typedef double ld;
	    typedef pair<ll, ll> pll;
	    typedef pair<int, int> pii;
	    #define F first
	    #define S second
	    #define all(x) x.begin(), x.end()
	    #define MP(x, y) make_pair(x, y)
	
	    const int maxn = 2e5 + 10;
	
	    int T = 1, n, k, a[maxn];
	
	    bool check(ll x) {
	        ll tmp = x;
	        int ptr = n;
	
	        for (int i = 1; i <= k; i++) {
	            while (ptr && a[ptr] > x) {
	                ptr--;
	            }
	
	            x -= ptr;
	        }
	
	        return (x == 0);
	    }
	
	    int main() {
	        cin >> T;
	
	        while (T--) {
	            cin >> n >> k;
	
	            for (int i = 1; i <= n; i++) {
	                cin >> a[i];
	            }
	
	            ll l = 0, r = 1e18;
	            while (r - l > 1) {
	                ll mid = (l + r) >> 1;
	
	                if (check(mid)) l = mid;
	                else r = mid;
	            }
	
	            cout << r << '\n';
	        }
	    }
		```

???+note "[CF EDU A. K-th Number in the Union of Segments](https://codeforces.com/edu/course/2/lesson/6/5/practice/contest/285084/problem/A)"
	有一個 multiset，$n$ 次 insert $l_i,\ldots ,r_i$ 進去，問 multiset 第 $k$ 小的數字（$k$ 為 0-base）　
	
	$n\le 50,0\le k\le 2\times 10^9,-2\cdot 10^9 \le l_i \le r_i \le 2\cdot 10^9$
	
	??? note "code"
		```cpp linenums="1"
		#pragma GCC optimize("O3,unroll-loops")
        #pragma GCC target("avx2,bmi,bmi2,lzcnt,popcnt")

        #include <bits/stdc++.h>
        #define pb push_back

        using namespace std;
        using ll = long long;

        struct Interval {
            int l, r;
        };

        int n, k;
        vector<Interval> intervals;

        bool check(ll t) {
            ll cnt = 0;
            for (auto &[l, r] : intervals) {
                if (t > r) {
                    cnt += r - l + 1;
                } else if (t > l) {
                    cnt += (ll)t - l;
                }
            }

            return cnt <= k;
        }

        signed main() {
            ios::sync_with_stdio(0);
            cin.tie(0);
            cin >> n >> k;
            for (int i = 0; i < n; i++) {
                int l, r;
                cin >> l >> r;
                intervals.pb({l, r});
            }

            ll l = -3e9, r = 3e9;
            // 找到第一個 x 滿足小於 x 的個數 <= k
            while (r - l > 1) {
                ll mid = (l + r) / 2;
                if (check(mid)) l = mid;
                else r = mid;
            }
            cout << l << '\n';
        } 
        ```