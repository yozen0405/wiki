- CF EDU
- <https://blog.csdn.net/qq_31908675/article/details/82749573?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168318044816800226588552%2522%252C%2522scm%2522%253A%252220140713.130102334.wap%255Fall.%2522%257D&request_id=168318044816800226588552&biz_id=0&utm_medium=distribute.wap_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-5-82749573-null-null.wap_first_rank_v2_rank_v29&utm_term=%E4%BA%8C%E5%88%86%20%E5%B0%BA%E5%8F%96&spm=1018.2118.3001.4187>



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
	    
???+note "[CSES - Sum of Four Values](https://cses.fi/problemset/task/1642/)"