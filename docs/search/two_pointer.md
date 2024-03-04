???+note "[CSES - Sum of Two Values](https://cses.fi/problemset/task/1640)"
	給一個長度為 n 的序列 $a_1, \ldots ,a_n$，以及數字 x，問這 n 個數字中哪兩個數字和為 x，輸出任何一組解
	

	$n\le 2\times 10^5, 1\le x,a_i\le 10^9$
	
	??? note "思路"
		排序後使用雙指針
		
	??? note "code"
		```cpp linenums="1"
		void solve() {
	        vector<pair<int, int>> v;
	        int n, x;
	        cin >> n >> x;
	        for (int i = 1; i <= n; i++) {
	            int x;
	            cin >> x;
	            v.push_back({x, i});
	        }
	        sort(v.begin(), v.end());
	        int l = 0, r = n - 1;
	        while (r > l) {
	            if (v[l].first + v[r].first > x) {
	                r--;
	            } else if (v[l].first + v[r].first < x) {
	                l++;
	            } else {
	                cout << v[l].S << ' ' << v[r].S << endl;
	                exit(0);
	            }
	        }
	        cout << "IMPOSSIBLE\n";
	    }
	    ```

???+note "[CSES - Sum of Three Values](https://cses.fi/problemset/task/1641)"
	給一個長度為 n 的序列 $a_1, \ldots ,a_n$，以及數字 x，問這 n 個數字中哪三個數字和為 x，輸出任何一組解
	
	$n\le 5000, 1\le x, a_i\le 10^9$
	
	??? note "思路"
		枚舉第一項，後續套用 Sum of Two Values

???+note "Sum of Two values 變化"
	給一個長度為 n 的序列 $a_1, \ldots ,a_n$，以及數字 x，問有幾組 (i, j) 使 $a_i+a_j=x$
	
	$n\le 2\times 10^5, 1\le x, a_i\le 10^9$
	
	??? note "思路"
		開一個桶 cnt[ ] 紀錄每種數字出現的次數，然後我們就可以將 a[ ] sort，並使用 unique 去除重複的元素，然後用雙指針（枚舉 l，r 會單調遞減）
		
	??? note "code"
		```cpp linenums="1"
		int j = n - 1;
		for (int i = 0; i < n; i++) {
			while (i < j && a[i] + a[j] >= x) {
				if (a[i] + a[j] == x) {
					ans += cnt[i] * cnt[j];
				}
				j--;
			}
		}
		```

???+note "[USACO 2021 December Contest, Silver Problem 2. Connecting Two Barns](https://www.usaco.org/index.php?page=viewproblem2&cpid=1159)"
	給 $n$ 點 $m$ 邊，點邊號 $1\ldots n$，可以額外建最多兩條邊，在點 $i,j$ 之間建邊花費 $(i-j)^2$，問最小花費使點 $1$ 跟點 $n$ 連通
	
	$n,m\le 10^5$
	
	??? note "思路"
		對於任何一個點，一定是選數值越接近自己的點越好，所以用 two pointer 維護，比自己小到第一個比自己大的數字（技巧，見代碼 line 54 ~ 68）
		
		講不清楚可以參考[官方詳解](https://www.usaco.org/current/data/sol_prob2_silver_dec21.html)
		
	??? note "code(from usaco)"
		```cpp linenums="1"
		#include <algorithm>
	    #include <iostream>
	    #include <numeric>
	    #include <vector>
	
	    using namespace std;
	
	    void dfs(const vector<vector<int>> &edges, vector<int> &component, const int currv, const int id) {
	        for (int child : edges[currv]) {
	            if (component[child] != id) {
	                component[child] = id;
	                dfs(edges, component, child, id);
	            }
	        }
	    }
	
	    void solve() {
	        int n, m;
	        cin >> n >> m;
	        vector<vector<int>> edges(n);
	        for (int i = 0; i < m; i++) {
	            int a, b;
	            cin >> a >> b;
	            a--;
	            b--;
	            edges[a].push_back(b);
	            edges[b].push_back(a);
	        }
	
	        vector<int> component(n);
	        iota(component.begin(), component.end(), 0);
	        for (int i = 0; i < n; i++) {
	            if (component[i] == i) {
	                dfs(edges, component, i, i);
	            }
	        }
	
	        if (component[0] == component[n - 1]) {
	            cout << "0\n";
	            return;
	        }
	
	        vector<vector<int>> componentToVertices(n);
	        for (int i = 0; i < n; i++) {
	            componentToVertices[component[i]].push_back(i);
	        }
	
	        long long ans = 1e18;
	        vector<long long> srccost(n, 1e9);
	        vector<long long> dstcost(n, 1e9);
	        int srcidx = 0;
	        int dstidx = 0;
	        for (int i = 0; i < n; i++) {
	            while (srcidx < componentToVertices[component[0]].size()) {
	                srccost[component[i]] = min(srccost[component[i]],
	                                            (long long)abs(i - componentToVertices[component[0]][srcidx]));
	                if (componentToVertices[component[0]][srcidx] < i) srcidx++;
	                else break;
	            }
	            if (srcidx) srcidx--;
	
	            while (dstidx < componentToVertices[component[n - 1]].size()) {
	                dstcost[component[i]] = min(dstcost[component[i]],
	                                            (long long)abs(i - componentToVertices[component[n - 1]][dstidx]));
	                if (componentToVertices[component[n - 1]][dstidx] < i) dstidx++;
	                else break;
	            }
	            if (dstidx) dstidx--;
	        }
	
	        for (int i = 0; i < n; i++) {
	            ans = min(ans, srccost[i] * srccost[i] + dstcost[i] * dstcost[i]);
	        }
	
	        cout << ans << "\n";
	    }
	
	    int main() {
	        ios_base::sync_with_stdio(false);
	        cin.tie(NULL);
	        int t;
	        cin >> t;
	
	        for (int i = 0; i < t; i++) {
	            solve();
	        }
	
	        return 0;
	    }
	    ```

???+note "[USACO 2013 JAN Cow Lineup G](https://www.luogu.com.cn/problem/P3069)"
	有 n 頭牛排成一列，其中第 i 個的品種是 a[i]。只能刪掉至多 k 種品種的情況下，問品種相同的連續段的最大長度
	
	$n\le 10^5, a_i \le 10^9$
	
	??? note "思路"
		可以把題目看成: 對於每個有 k + 1 種品種的 subarray，問同種種類最多可以是多少。
		
		最暴力的想法就是枚舉右界 r，然後暴力的找到 l 使 [l, r] 恰有 k + 1 種品種，用 r 的品種來更新答案。但我們可以發現，這種 subarray 具有單調性，我們可以用 two pointer 維護，詳見代碼。
		
	??? note "code"
		```cpp linenums="1"
		#include <iostream>
	    #include <map>
	
	    using namespace std;
	
	    const int N = 100005;
	
	    int a[N];
	
	    int main() {
	        int n, k;
	        cin >> n >> k;
	
	        for (int i = 1; i <= n; ++i) {
	            cin >> a[i];
	        }
	
	        map<int, int> mp;
	        int ans = 0;
	        int l = 1;
	        for (int r = 1; r <= n; ++r) {
	            ++mp[a[r]];
	
	            while (mp.size() > k + 1) {
	                --mp[a[l]];
	                if (mp[a[l]] == 0) {
	                    mp.erase(a[l]);
	                }
	                ++l;
	            }
	
	            ans = max(ans, mp[a[r]]);
	        }
	
	        cout << ans;
	    }
		```
		
???+note "[USACO 2019 FEB Sleepy Cow Herding S](https://www.luogu.com.cn/problem/P5541)"
	一維數線上有 n 頭牛，每次只能挪動 edge point（最右邊或最左邊）的牛到任意位置，不過不能使他移動後還是在 edge point，問讓這些牛完全相鄰的最少和最多挪動次數。

	$n\le 10^5$
	
	??? note "思路"
		對於第一個問題求最小操作次數，由於每一步操作都在佔領空位，而最終狀態為一段包含連續 $n$ 個位置的區間，所以可以從結果出發，用 two pointer，枚舉這個最終區間的左端點 $a_i$，找到右端點長度 $a_j$，使 $a_j-a_i+1\le n$，這段區間的答案就會是 n - 區間內的牛的數量（將外面的牛都移進來這個區間內），即 n - (i - j + 1)。可以看一下下圖的移動方式。
		
		<figure markdown>
          ![Image title](./images/9.png){ width="300" }
        </figure>

        因為我們 edge point 移動過去之所以合法是因為我們能找到中間的空格，或者是另一端也有 edge point，可以讓牛安心的過去另一端 edge point 的旁邊。如果兩者都沒有，就會是以下兩種特殊情況。如果前 $n-1$ 個位置緊鄰，而最後一個位置離倒數第二個位置距離大於 $2$，比如 $1,2,3,4,7$，答案應為 $2$。同理，如果後 $n-1$ 個位置緊鄰，而第一個位置離第二個位置距離大於 $2$，答案也應為 $2$。

        <figure markdown>
          ![Image title](./images/10.png){ width="400" }
        </figure>

        因為不能從 edge point 還到 edge point ，所以會比較類似一個區間一直在縮小（一個大的區間縮小成為一個長度為 n 的區間）。由於要讓移動次數越大越好，所以我們要盡量慢慢移動，而收攏的區間一定是越大越好，所以我們可以朝最左邊慢慢收攏過去，或是朝最右邊慢慢收攏過去。假如現在是朝最左邊慢慢收攏過去，我們一開始先將第 n 頭牛移動到區間 [a[1], a[n - 1]] 的最右邊的空格，這樣才不會讓他成為 edge point，然後再來我們就只要計算 [a[1], a[n - 1]] 內空格的數量，就可以得知接下來的操作次數。假如區間是 [l, r]（在這邊 l = a[1], r = a[n - 1]），答案也就是區間長度 - 區間內的牛的數量 + 一開始第 n 頭牛移過去的一次操作 = (r - l + 1) - n + 1。同理朝最右邊慢慢收攏過去就是 l = a[2], r = a[n]。
        
        <figure markdown>
          ![Image title](./images/11.png){ width="300" }
        </figure>
		
	??? note "code"
        ```cpp linenums="1"
        #include <bits/stdc++.h>
        using namespace std;

        int n, a[100005], ans, ans2;

        int main() {
            cin >> n;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }
            sort(a + 1, a + n + 1);
            if ((a[n - 1] - a[1] == n - 2 && a[n] - a[n - 1] > 2) || (a[n] - a[2] == n - 2 && a[2] - a[1] > 2)) {
                ans = 2;  // 特判
            } else {
                int j = 1, res = 0;
                for (int i = 1; i <= n; i++) {
                    while (j < n && a[j + 1] - a[i] + 1 <= n) {
                        j++;
                    }
                    res = max(res, j - i + 1);
                }
                ans = n - res;
            }
            cout << ans << '\n';
            cout << max(a[n - 1] - a[1], a[n] - a[2]) - n + 2 << '\n';
            return 0;
        }
        ```