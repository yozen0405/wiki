## 模板

???+note "code"
	```cpp linenums="1"
	struct Node {
        Node *ch[26];
        int idx = 0;
    };

    void add (Node* rt, string s, int idx) {
        for (int i = 0; i < s.size(); i++) {
            int c = s[i] - 'a';
            if (rt->ch[c] == nullptr) {
                rt->ch[c] = new node();
            }
            rt = rt->ch[c];
        } 
        rt->idx = idx;
    }
    ```

## 例題

???+note "[CSES - Word Combinations](https://cses.fi/problemset/task/1731/)"
	給長度為 $n$ 字串 $S$ 及 $k$ 個字串 $T_i$，問有多少種組合可以組出目標字串可（重複使用）
	
	$n\le 5000,k\le 10^5,\sum |T_i| \le 10^6$
	
	??? note "思路"
		使用動態規劃 : 
		
		- 狀態 $dp(i) =$ 要組合出 $S(1, i)$ 的方法數
	
		- 轉移 $dp(i) = \sum dp(i - |T_j|)$ 若 $T_j = S(i - |T_j| + 1, i)$
		
		如何快速判斷 $T_j$ 和 $S(i - |T_j| + 1, i)$ 是否相等 ? 使用 Trie 幫助轉移。先建出每個 $T_i$ 反向字串的 Trie，轉移過程按照 $S$ 走訪 Trie，若遇到單字則轉移。時間複雜度 : 狀態數 $O(n)$，轉移為 $O(n)$，共 $O(n^2)$
	
	??? note "code"
		```cpp linenums="1"
	    #include <bits/stdc++.h>
	    #define int long long
	    #define lowbit(x) (x & (-x))
	    #define IO ios::sync_with_stdio(0);cin.tie(0);
	    #define pii pair<int, int>
	    #define mk make_pair
	    #define pb push_back
	    using namespace std;
	
	    const int INF = 0x3f3f3f3f;
	    const int maxn = 1e6 + 5;
	    const int M = 1e9 + 7;
	    int dp[maxn];
	
	    struct node {
	        node *ch[26];
	        int idx = 0;
	    };
	
	    void add (node *rt, string s, int idx) {
	        for (int i = 0; i < s.size(); i++) {
	            int c = s[i] - 'a';
	            if (rt -> ch[c] == nullptr) {
	                rt -> ch[c] = new node();
	            }
	            rt = rt -> ch[c];
	        } 
	        rt -> idx = idx;
	    }
	
	    void solve (node *rt, int idx, string str) {
	        for (int i = idx; i >= 1; i--) {
	            rt = rt -> ch[str[i - 1] - 'a'];  
	            if (rt == nullptr) break;
	            if (rt -> idx) {
	                dp[idx] += dp[i - 1];
	                dp[idx] %= M;
	            }
	        }
	    }
	
	    signed main () {
	        string str;
	        int n;
	        cin >> str;
	        cin >> n;
	        vector<string> s(n + 1);
	        node *rt = new node();
	        for (int i = 1; i <= n; i++) {
	            cin >> s[i];
	            reverse(s[i].begin(), s[i].end());
	            add(rt, s[i], i);
	        }
	        dp[0] = 1;
	        for (int i = 1; i <= str.size(); i++) {
	            solve(rt, i, str);
	        }
	        cout << dp[str.size()] << "\n";
	    }
	    ```

## 0-1 Trie

將數字轉成二進位，當成字串打到 Trie 上面

<figure markdown>
  ![Image title](./images/4.png){ width="500" }
</figure>

### 最大異或數對

???+note "[LOJ #10050. 「一本通 2.3 例 2」The XOR Largest Pair](https://loj.ac/p/10050)"
	給 $N$ 個數字 $a_i$，找出其中兩個數字使得兩數 xor 數值最大
	
	$1\le N\le 10^5,0\le A_i < 2^{31}$
	
	??? note "思路"
		將 $a$ 打到 01 Trie 上，對於每個數字從 root Greedy 的走下去

???+ note "變化題 K-th Maximum XOR of Two Numbers in an Array"
	給長度為 $n$ 的陣列 $a$，問兩個元素 xor 起來，第 $k$ 大是多少
	
	$n\le 10^5$
	
	??? note "想法"
	    - 二分搜 $O(\log C)$
	
	    - 用 $\texttt{Trie}$ 檢查 $O(n\log C)$ 
	        - 對於每個 $a_i$ 找 $a_i \oplus a_j \le x$ 的 $a_j$ 有幾個
	        - 每次在 $\texttt{Trie}$ 上 $\texttt{find }O(\log C)$ (深度)
	        - 有 $n$ 個 $a_i$ 所以才是 $O(n\log C)$
	        - $\Rightarrow O(n\log^2 C)$

### 最大異或路徑		

???+note "[LOJ #10056. 「一本通 2.3 练习 5」The XOR-longest Path](https://loj.ac/p/10056)"
	給定一棵 n 個點的帶權樹，求樹上最長的異或和路徑。
	
	$1\le n\le 10^5, 0\le w < 2^{31}$
	
	??? note "思路"
		$f(u,v)=f(rt,u)\oplus f(rt,v)$
		
		問題就轉成挑兩個數起來最大的

???+ note "變化題 [CF 1055 F. Tree and Xor](https://codeforces.com/contest/1055/problem/F)"
    給一顆 $n$ 個點樹，設 $f(u,v)$ 為 $u$ 到 $v$ 的邊權異或和，問對於所有的 $f(u,v)$ 第 $k$ 大是多少
    
    $n\le 2\times 10^5$
    
    ??? note "想法"
        - k-th Xor path problem
        - $f(u,v)=f(rt,u)\oplus f(rt,v)$
        - 問題就轉成挑兩個 XOR 起來第 $k$ 大

### 習題

???+ note "延[CSES - Maximum Xor Subarray](https://cses.fi/problemset/task/1655/)"
	給長度為 $n$ 的陣列 $a$，最大 xor 起來的 Subrray 是多少
	
	$n\le 2\times 10^5,0\le x_i\le 10^9$
	
	??? note "想法"
		S[i, j] = S[j] ^ S[i - 1]，就變成上面最大異或數對的問題了
	 
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
	    using PQ = priority_queue<int, vector<int>, greater<int>>;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    struct node {
	        node *ch[2];
	
	        vector<int> con (int x) {
	            vector<int> res;
	            for (int i = 0; i <= 30; i++) {
	                if (x & (1 << i)) res.pb (1);
	                else res.pb (0);
	            }
	            return res;
	        }
	
	        void add (int x, node *rt) {
	            vector<int> res = con (x);
	            int n = res.size ();
	
	            for (int i = n - 1; i >= 0; i--) {
	                if (rt -> ch[res[i]] == nullptr) rt -> ch[res[i]] = new node ();
	                rt = rt -> ch[res[i]];
	            }
	        }
	
	        int find (int x, node *rt) {
	            vector<int> res = con (x);
	            int n = res.size();
	
	            int ret = 0;
	            for (int i = n - 1; i >= 0; i--) {
	                if (rt -> ch[res[i] ^ 1] == nullptr) rt = rt -> ch[res[i]], ret += (res[i] << i);
	                else rt = rt -> ch[res[i] ^ 1], ret += ((res[i] ^ 1) << i);
	            }
	            return ret;
	        }
	    };
	
	    int n;
	    int a[maxn], pre[maxn];
	
	    void init () {
	        cin >> n;
	        for (int i = 1; i <= n; i++) cin >> a[i];
	    }
	
	    void solve () {
	        node *rt = new node ();
	
	        int res = 0;
	        rt -> add (0, rt);
	        for(int i = 1; i <= n; i++) {
	            pre[i] = pre[i - 1] ^ a[i];
	            int ret = rt -> find (pre[i], rt) ^ pre[i];
	            res = max (res, ret);
	            rt -> add (pre[i], rt);
	        }
	
	        cout << res << "\n";
	    } 
	
	    signed main() {
	        // ios::sync_with_stdio(0);
	        // cin.tie(0);
	        int t = 1;
	        //cin >> t;
	        while (t--) {
	            init();
	            solve();
	        }
	    } 
	    ```

???+note "[USACO 2019 Dec. Gold p1](http://www.usaco.org/index.php?page=viewproblem2&cpid=921)"
    給一顆樹，賦予每個 Node $a_i$，$q$ 筆詢問
    
    - $\text{modify}(u,val):$ 把 $a_u = val$
    
    - $\text{query}(u,v):$ 問把 $u \rightarrow v$ 的 path 上的$a_i \texttt{ XOR}$ 起來是多少
    
    ??? note "解析"
        - 相關的問題(不是 Trie)
        
        - $f(u,v)=f(u,rt) \oplus f(v,rt)$
    
        - 問題就轉成了 CSES path queries I
    
        - 用 euler technique 解決