> https://www.youtube.com/watch?v=jqJ5s077OKo

https://drive.google.com/file/d/1tzGX9PH0UJiER01kPtUTFJnyMLpfR877/view

https://tioj.ck.tp.edu.tw/problems/2227

## 例題

### CSES Beautiful Subgrids

???+note "[CSES - Beautiful Subgrids](https://cses.fi/problemset/task/2137)"
	給一個 $n\times n$ 的 grid，每一格非黑即白，問有幾個子矩形滿足 :
	
	- 至少 $2\times 2$

	- 四個角落都是黑色的

	$n\le 3000$

	??? note "思路"
		先想暴力，暴力的話一定是枚舉 $x_1,y_1,x_2,y_2$，這樣 $O(n^4)$。$O(n^2)$ 的話先列舉 $y_1, y_2$，然後 $O(n)$ 找合法的 $x$。
		
		至於要怎麼 $O(n)$ 找合法的 $x$ : (bitset[y1] & bitset[y2]).count()
		
		複雜度 $O(\frac{n^3}{64})$

### USACO Lots of Triangles

???+note "[USACO 2016 Dec. Platinum P1. Lots of Triangles](https://www.usaco.org/index.php?page=viewproblem2&cpid=672)"
	給 $n$ 的二維座標點，任三點不共直線，任三點可以形成三角形。對每個點，輸出他會被包含在幾個三角形內
	
	$n\le 300$
	
### 全國賽 共同朋友

???+note "[2021 全國賽 pE. 共同朋友](https://tioj.ck.tp.edu.tw/problems/2227)"
	有 $n$ 個人，對於第 $i$ 個人給出他的 $d_i$ 個朋友，問有多少組 $(a,b)$ 滿足 $a<b$ 且 $a$ 與 $b$ 有共同的朋友
	
	$n\le 2500$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pii pair<int, int>
        #define pb push_back
        using namespace std;

        const int maxn = 2505;
        const int M = 1e9 + 7;
        const int INF = 0x3f3f3f3f;

        bitset<maxn> B[maxn];
        int n, m;

        void init () {
            cin >> n;
            for (int i = 1, u, v; i <= n; i++) {
                int t;
                cin >> t;
                while (t--) {
                    cin >> v;
                    v++;
                    B[i][v] = 1;
                }
            }
        }

        void solve () {
            bitset<maxn> b; 
            int res = 0;
            for (int i = 1; i <= n; i++) {
                for (int j = i + 1; j <= n; j++) {
                    b = B[i] & B[j];
                    if (b.count()) res++;
                }
            }
            cout << res << "\n";
        }

        signed main () {
            init();
            solve();
        }
		```
	
### 背包問題

答案 true false 的可以思考看看能不能用位元運算

#### 0/1 背包

???+note "0/1 背包"
	給定 $N$ 個物品，第 $i$ 個物品重量 $w_i$，問是否能選一些物品使重量總和湊到 $W$ 
	
	??? note "思路"
		一般我們會這樣寫，複雜度 $O(nW)$
		
		```cpp linenums="1"
		bool dp[maxW];
		int main() {
			int n, W;
			cin >> n >> W;
			dp[0] = true;
			for(int id = 0; id < n; id++) {
				int x;
				cin >> x;
				for(int i = W; i >= x; i--) {
					if(dp[i - x]) dp[i] = true;
				}
			}
			puts(dp[W] ? "YES" : "NO");
		}
		```
		
		我們可以將內層的迴圈改成 `dp |= (dp << x)`，如下
		
		```cpp linenums="1"
		bitset<maxW> dp;
		int main() {
			int n, W;
			cin >> n >> W;
			dp[0] = true;
			for(int id = 0; id < n; id++) {
				int x;
				cin >> x;
				dp |= (dp << x);
			}
			puts(dp[W] ? "YES" : "NO");
		}
		```
		
		這樣做之後複雜度為 $O(\frac{nW}{64})$

???+note "[TIOJ 1993. 冰塊塔](https://tioj.ck.tp.edu.tw/problems/1993)"
	給 $n$ 個 tuple $(x_i,y_i,z_i)$，對於每個 tuple 選 $x_i,y_i,z_i$ 其中一個放入背包裡，使在背包重量不超過 $W$ 的情況下，最大化背包重量總和
	
	有 $t$ 組測資
	
	$t\le 20,n\le 10^3,W\le 10^5,x_i,y_i,z_i\le 10^4$
	
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
        const int maxn = 1e5 + 5;

        bitset<maxn> dp;

        void solve() {
            int n, W;
            cin >> n >> W;
            dp.reset();
            dp.set(0);
            for (int i = 0; i < n; i++) {
                int x, y, z;
                cin >> x >> y >> z;
                dp = (dp << x) | (dp << y) | (dp << z);
            }

            for (int i = W; i > 0; i--) {
                if (dp[i]) {
                    cout << i << '\n';
                    return;
                }
            }
            cout << "no solution\n";
        }

        signed main() {
            int t = 1;
            cin >> t;
            while (t--) {
                solve();
            }
        }   
        ```
	
#### 有限背包

???+note "有限背包"
	給定 $N$ 個物品，第 $i$ 個物品有 $c_i$ 個，每個重量 $w_i$，問是否能選一些物品使重量總和湊到 $W$ 

	??? note "思路"
		把 $c_i$ 分成 $\log c_i$ 組，用 0/1 背包做
		
		複雜度 $O(\frac{n\log c_i\times W}{64})$

#### 無限背包

???+note "無限背包"
	給定 $N$ 個物品，第 $i$ 個物品有無限多個，每個重量 $w_i$，問是否能選一些物品使重量總和湊到 $W$ 
	
	??? note "思路"
		設 $c_i = W / w_i$，當有限背包解

		複雜度 $O(\frac{n\log W\times W}{64})$