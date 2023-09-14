
		
???+note "[TIOJ  2221 . 足球場](https://tioj.ck.tp.edu.tw/problems/2221)"
	給 $n$ 個二維座標點，問能組成多少個矩形
	
	$1\le n\le 1000,0\le x_i, y_i \le 10^9$
	
	??? note "思路"
		矩形由「對角線長度」和「中心座標」決定。所以我們枚舉 $i,j$，將 pair(i 跟 j 的距離, i 跟 j 的中點)++，最後對於每個 distinct pair 的方法數就是 $C^k_2$
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define StarBurstStream ios_base::sync_with_stdio(false); cin.tie(0);
        #define mp(a, b) make_pair(a, b)
        #define F first
        #define S second
        using namespace std;
        typedef long long ll;
        using pll = pair<ll, ll>;

        ll dis(pll a, pll b){
            ll x = a.F - b.F;
            ll y = a.S - b.S;
            return x * x + y * y;
        }

        pll operator+(pll a, pll b){
            return mp(a.F + b.F, a.S + b.S);
        }

        int main(){
            StarBurstStream

            int n;
            cin >> n;

            vector<pll> p(n);
            for(int i = 0; i < n; i++){
                cin >> p[i].F >> p[i].S;
            }

            map<pair<pll, ll>, ll> cnt;

            for(int i = 0; i < n; i++){
                for(int j = i + 1; j < n; j++){
                    cnt[mp(p[i] + p[j], dis(p[i], p[j]))]++;
                }
            }

            ll ans = 0;
            for(auto i : cnt){
                ans += i.S * (i.S - 1) / 2;
            }
            cout << ans << "\n";

            return 0;
        }
        ```
        
???+note "[Atcoder abc218 D. Rectangles](https://atcoder.jp/contests/abc218/tasks/abc218_d)"
	給 $n$ 個二維座標點，問能組成多少個矩形滿足該矩形平行 $x$ 軸和 $y$ 軸
	
	$4\le n\le 2000,0\le x_i,y_i\le 10^9$
	
	??? note "思路"
		跟上一題的差別是我們在枚舉 i, j 時需要固定一個維度，然後去做一樣的事情
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pb push_back
        #define mk make_pair
        #define F first
        #define S second
        #define ALL(x) x.begin(), x.end()

        using namespace std;
        using pii = pair<int, int>;

        struct Node {
            int x, y;
        };

        const int N = 2005;
        int n;
        Node a[N];

        signed main() {
            cin >> n;
            for (int i = 0; i < n; i++) {
                cin >> a[i].x >> a[i].y;
            }
            map<pii, int> mp;
            int ans = 0;
            for (int i = 0; i < n; i++) {
                for (int j = i + 1; j < n; j++) {
                    if (a[i].y == a[j].y) {
                        int len = abs(a[j].x - a[i].x);
                        int point = (a[i].x + a[j].x);
                        mp[{len, point}]++;
                    }
                }
            }
            int ans = 0;
            for (auto it : mp) {
                ans += it.S * (it.S - 1) / 2;
            }
            cout << ans << '\n';
        } 
        ```