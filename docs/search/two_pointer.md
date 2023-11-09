- CF EDU

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