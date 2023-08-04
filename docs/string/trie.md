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
		
### 最大異或路徑		

???+note "[LOJ #10056. 「一本通 2.3 练习 5」The XOR-longest Path](https://loj.ac/p/10056)"
	