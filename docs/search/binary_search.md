- 第 k 大

- noi 2016

- 平均 

- 最大化最小值

- 雙層二分搜(Google code jam)

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

???+note "[CF 1853 C. Ntarsis' Set](https://codeforces.com/contest/1853/problem/C)"
	給一個包含 $1,2,\ldots ,10^{1000}$ 所有數字的 Set $S$，每天從 $S$ **同時**移除第 $a_1,a_2,\ldots ,a_n$ 個數字，問 $k$ 天之後 $S$ 中最小的數字是多少
	
	$n,k\le 2\times 10^5,1\le a_i \le 10^9,a_1<a_2<\ldots < a_n$
	
	??? note "思路"
		二分搜最後一個在「移除區段」的數字
		
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
