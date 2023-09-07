???+note "[2023 TOI 4 模 p2. 整除序列 (Divide)](https://drive.google.com/file/d/1CvB1-JyGmFaKq1Ir8VJ5avx7NUFT2Hl9/view)"
	有一個長度為 $n$ 的陣列 $a_1,\ldots, a_n$，你只知道每個數字的其中一個 character，要構造出陣列，使得相鄰的兩個數字互為因數或倍數關係，且補上的 character 數量要越少越好
	
	$2\le n\le 10^5$
	
	??? note "思路"
		可以觀察到每一項的數字都不會太大，所以可以先猜一個 threshold T
		
		然後去 dp(i, x) : 0~i 要讓 a[i] = x 最少要填的 character 數量
		
		f(i, k) : 0~i 要讓 a[i]|k 或 k|a[i] 最少要填的 character 數量
		
		f(i, k) = min{dp(i, x) + size(x) - 1} 其中 x|k or k|x 且 dp(i, x) 有解
		
		dp(i + 1, k) = 
		
		- k 有 b[i + 1]: f(i, k) + size(k) - 1
	
		- otherwise: -1
	
		可以先預處理 0~T 每個數字會不會出現某個 character exist[x][0~9]。這樣對於每個 i 都要做一次篩法從 dp(i, x) 轉移到 f(i, k)，複雜度 O(n * T * log(T))。

???+note "[CF 1860 D. Balanced String](https://codeforces.com/contest/1860/problem/D)"
	給你一個由 01 組成的字串 s，在一次操作下，你可以 swap 任意兩項。問最少幾次操作可讓 s 的逆序數對數量（10） = 正序數對數量（01）
	
	$3\le |s| \le 100$
	
	??? note "思路"
		先想考慮將這個字串 ramdom shuffle 後，怎麼計算最少要 swap 幾次 ? 其實只要看 a[i] != b[i] 的數量除以 2 即可（因為每次 swap 一定可以換好兩個）
	
		dp[i][j][k] : 代表到第 i 個位置為止共放了 j 個 1，10 數量為 k，跟原本的 a[i] != b[i] 的數量最少有幾個
		
		dp[i][j][k] 轉移到 :
	    
	    - 放 0 : dp[i+1][j+1][k + j]
	
	    - 放 1 : dp[i+1][j][k]
		
		這樣可能會 MLE，考慮到 dp[i + 1][ ][ ] 只用到 dp[i][ ][ ]，所以可以滾動。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	    #define ll long long
	    #define pb push_back
	    #define all(x) (x).begin(), (x).end()
	    #define pii pair<int, int>
	    const int mod = 998244353, N = 105;
	
	    int dp[2][N][N * N];
	
	    void solve() {
	        string s; cin >> s;
	        int n = s.length();
	        int tot = 0, cnt0 = 0, cnt1 = 0;
	        for (char c : s) {
	            (c == '0' ? cnt0 : cnt1)++;
	        }
	        tot = n * (n - 1) / 2;
	        tot -= cnt0 * (cnt0 - 1) / 2;
	        tot -= cnt1 * (cnt1 - 1) / 2;
	        assert(tot % 2 == 0);
	
	        for (int i = 0; i < 2; ++i) {
	            for (int j = 0; j < N; ++j) {
	                for (int k = 0; k < N * N; ++k) {
	                    dp[i][j][k] = 1 << 30;
	                }
	            }
	        }
	
	        int now = 0;
	        dp[1][0][0] = 0;
	        for (int i = 0; i < n; ++i) {
	            for (int j = 0; j < N; ++j) {
	                for (int k = 0; k < N * N; ++k) {
	                    dp[now][j][k] = 1 << 30;
	                }
	            }
	            for (int j = 0; j <= n; ++j) {
	                for (int k = 0; k <= n * (n - 1) / 2; ++k) {
	                    if (dp[now ^ 1][j][k] <= n) {
	                        int c0 = (s[i] == '1'), c1 = c0 ^ 1;
	                        // 0
	                        dp[now][j][k + j] = min(dp[now][j][k + j], dp[now ^ 1][j][k] + c0);
	                        // 1
	                        dp[now][j + 1][k] = min(dp[now][j + 1][k], dp[now ^ 1][j][k] + c1);
	                    }
	                }
	            }
	            now ^= 1;
	        }
	        cout << dp[now ^ 1][cnt1][tot / 2] / 2 << '\n';
	    }
	
	    int main() {
	        ios::sync_with_stdio(false), cin.tie(0);
	        int t = 1;
	        // cin >> t;
	        while (t--) {
	            solve();
	        }
	    }
	    ```

???+note "[CF 1861 E. Non-Intersecting Subpermutations](https://codeforces.com/contest/1861/problem/E)"
	給 $n,k$，對於一個長度為 $n$，每個元素皆在 $1\ldots k$ 的 array，cost 的計算方式為「最多可切出幾段 $1\ldots k$ 的 permutation」，輸出所有可能的 array 的 cost 總合為多少
	
	$2\le k\le n\le 4000$
	
	??? note "思路"
		想法 : 對於一個 subarray，貢獻了幾個 array
	
	    dp[i] = 有幾個長度為 i 的 array 滿足 subarray(i - k + 1, i) 是有貢獻到的
	
	    [i - k + 1, i] 一定要是一個 1~k 的 permutation，而之前的每一格我們擺 1~k 都可以，所以 initially dp[i] = k! * exp(k, i - k)
	
	    考慮要從 dp[i] 中扣掉不合法的 case，也就是有幾個 permutation 的結尾在 [i - k + 1, i) 就已形成。設 j $\in$ [i- k + 1, i)，我們需要將 dp[i] -= dp[j] * (i - j)!
	
	    最後，對於每個 i，因為後面可以隨便擺，對答案的貢獻就是 dp[i] * exp(k, n - i)
	    
	    > 參考自 : <https://codeforces.com/blog/entry/119891?#comment-1064151>
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    const int N = 4005;
	    const int M = 998244353;
	
	    int n, k;
	    int dp[N], fac[N], powK[N];
	
	    void init() {
	        powK[0] = 1;
	        fac[0] = 1;
	        for (int i = 1; i <= n; i++) {
	            powK[i] = (powK[i - 1] * k) % M;
	            fac[i] = (fac[i - 1] * i) % M;
	        }
	    }
	
	    signed main() {
	        cin >> n >> k;
	        init();
	
	        for (int i = k; i <= n; i++) {
	            dp[i] = (fac[k] * powK[i - k]) % M;
	            for (int j = i - k + 1; j < i; j++) {
	                dp[i] = (dp[i] - (dp[j] * fac[i - j]) % M + M) % M;
	            }
	        }
	
	        int ans = 0;
	        for (int i = k; i <= n; i++) {
	            ans = (ans + dp[i] * powK[n - i]) % M;
	        }
	        cout << ans << '\n';
	    }
	    ```

???+note "<a href="/wiki/dp/images/ionc_307.html" target="_blank">2022 IONC Day3 G. TypeRacer 2 (typeracer2)</a>"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，鍵盤左到右是 $1, 2, \ldots, k$，兩隻手指頭一開始可放任意位置。手指從 $i$ 到 $j$ 會花 $|i-j|$，輸出將陣列 $a$ 打完的最少時間
	
	$1\le n, k\le 2\times 10^5$
	
	??? note "思路"
		dp(i, j) = 每次只動一隻手的條件下，打完第 i 個鍵，另隻手在 j 的最小 cost
	
	    $dp(i,j) \to \begin{cases}dp(i+1, j) ,\space \text{cost}(a_{i}, a_{i+1}) \\ dp(i + 1, a_{i}) ,\space \text{cost}(j, a_{i+1}) \end{cases}$
	    
	    basecase: dp(1, 1~k) = 0, 其他 = INF
	    
	    ---
	    
	    > 實作: 資料結構優化
	    
	    我們把 $dp(i + 1, a_{i})$ 單獨拉出來看
	
	    $dp(i+1, a_i)=\min \limits_{j=1\ldots k} \{dp(i, j) + |a_{i+1} - j| \}$
	
	    $dp(i + 1, a_i) = \min \begin{cases} dp(i, j) + a_{i+1} - j \space \forall j \le a_{i+1} \\  dp(i, j) +j - a_{i+1} \space \forall j > a_{i+1} \end{cases}$
	
	    所以我們只需要去維護兩顆線段樹 $dp(i,j)+j, dp(i, j) - j$ 即可
	    
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pb push_back
	    using namespace std;
	
	    const int INF = 2e18;
	
	    struct Node {
	        Node *lc = nullptr;
	        Node *rc = nullptr;
	        int l, r;
	        int add = 0, mn = 0;
	
	        Node(int l, int r) : l(l), r(r) {} 
	
	        void pull() {
	            mn = min(lc->mn, rc->mn);
	        }
	        void push() {
	            if (add) {
	                lc->add += add;
	                lc->mn += add;
	                rc->add += add;
	                rc->mn += add;
	                add = 0;
	            }
	        }
	    };
	
	    Node* build(int l, int r) {
	        Node *root = new Node(l, r);
	        if (l == r) {
	            return root;
	        }
	        int mid = (l + r) / 2;
	        root->lc = build(l, mid);
	        root->rc = build(mid + 1, r);
	        root->pull();
	        return root;
	    }
	
	    void add(Node *root, int ml, int mr, int val) {
	        if (ml > mr) return;
	        if (ml <= root->l && root->r <= mr) {
	            root->add += val;
	            root->mn += val;
	            return;
	        } 
	        root->push();
	        if (ml <= root->lc->r) {
	            add(root->lc, ml, mr, val);
	        }
	        if (root->rc->l <= mr) {
	            add(root->rc, ml, mr, val);
	        }
	        root->pull();
	    }
	
	    int query(Node *root, int ql, int qr) {
	        if (ql > qr) return INF;
	        if (ql <= root->l && root->r <= qr) {
	            return root->mn;
	        }
	        root->push();
	        int ret = INF;
	        if (ql <= root->lc->r) {
	            ret = min(query(root->lc, ql, qr), ret);
	        }
	        if (root->rc->l <= qr) {
	            ret = min(query(root->rc, ql, qr), ret);
	        }
	        root->pull();
	        return ret;
	    }
	
	    const int N = 2e5 + 5;
	    int n, k;
	    int a[N];
	    signed main () {
	        cin >> n >> k;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i];
	        }
	        Node *root_del = build(1, k);
	        Node *root_add = build(1, k);
	        for (int i = 1; i <= k; i++) {
	            add(root_del, i, i, -i);
	            add(root_add, i, i, +i);
	        }
	        for (int i = 1; i < n; i++) {
	            // calculate dp(i + 1, a[i])
	            int dp = min(query(root_del, 1, a[i + 1]) + a[i + 1], 
	                         query(root_add, a[i + 1] + 1, k) - a[i + 1]);
	
	            // update dp(i + 1, *)
	            add(root_del, 1, k, abs(a[i + 1] - a[i]));
	            add(root_add, 1, k, abs(a[i + 1] - a[i]));
	            // update dp(i + 1, a[i])
	            int now = query(root_del, a[i], a[i]) + a[i];
	            if (dp < now) {
	                add(root_del, a[i], a[i], -now+dp);
	                add(root_add, a[i], a[i], -now+dp);
	            }
	        }
	        int ans = INF;
	        for (int i = 1; i <= k; i++) {
	            ans = min(ans, query(root_del, i, i) + i);
	        }
	        cout << ans << '\n';
	    }
	    ```

???+note "[TOI 2022 B. 打鍵盤 (keyboard)](https://tioj.ck.tp.edu.tw/problems/2247)"
	給一個長度為 $n$ 的字串 $S$，一開始左手指在 F，右手指在 J，每次可將一隻手指移動一單位，輸出將字串 $S$ 打完的最少次數
	
	$1\le n\le 10^4,S$ 僅由英文大寫字母構成
	
	??? note "思路"
		先利用 Floyd Warshall 建好 dis(A-Z, A-Z)
		
		dp(i, j) = 每次只動一隻手的條件下，打完第 i 個鍵，另隻手在 j 的最小 cost
	
	    $dp(i,j) \to \begin{cases}dp(i+1, j) ,\space \text{cost}(a_{i}, a_{i+1}) \\ dp(i + 1, a_{i}) ,\space \text{cost}(j, a_{i+1}) \end{cases}$
		
		轉移從 dp(i, * ) 推到 dp(i + 1, * )，時間複雜度 O(26n)

### CF 484D

???+note "[CF 484 D. Kindergarten](https://codeforces.com/problemset/problem/484/D)"
	給一個長度為 $n$ 的陣列 $a_1, \ldots ,a_n$，能將陣列切成好幾段，問每段的 max - min 加起來最大是多少
	
	$1\le n\le 10^6, -10^9 \le a_i \le 10^9$
	
	??? note "思路"
		我們可以發現，將陣列分成好幾段，若遇到轉折就切，一定是最好的。感性理解的話，就是將每個能用差值都用上
		
		<figure markdown>
	      ![Image title](./images/15.png){ width="300" }
	    </figure>
	    
	    但在轉折處，會有一段貢獻不會選到，我們就要用 dp 到底計算選哪個比較好
	    
	    <figure markdown>
	      ![Image title](./images/16.png){ width="300" }
	    </figure>
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    using namespace std;
	
	    const int maxn = 1e6 + 5;
	    int n;
	    int a[maxn], dp[maxn];
	
	    void init () {
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i];
	        }
	    }
	
	    void solve () {
	        int j = 1; 
	        for (int i = 2; i <= n; i++) {
	            dp[i] = max(dp[j] + abs(a[i] - a[j + 1]), dp[j - 1] + abs(a[i] - a[j]));
	            if (a[i - 1] <= a[i] && a[i] >= a[i + 1]) j = i;
	            else if (a[i - 1] >= a[i] && a[i] <= a[i + 1]) j = i;
	        }
	        cout << dp[n] << "\n";
	    }
	
	    signed main () {
	        init();
	        solve();
	    }
		```
		
???+note "[CS Academy - Distinct Neighbours](https://csacademy.com/contest/archive/task/distinct_neighbours)"
	給你一個長度為 $n$ 的陣列 $a_1, a_2, \ldots ,a_n$，問有幾種 $a$ 的 permutation 滿足相同數字不相鄰
	
	$1\le n\le 750,1\le a_i \le n$
	
	??? note "思路"
		枚舉 k，假設有 k 個 gaps 至少有放一個東西，要將 cnt[i + 1] 個物品填入，利用隔板法得知方法數為 C(cnt[i + 1] - 1, k - 1)
		
		枚舉 L，代表選 L 個同 pair gaps，方法數 : C(j, L)
		
		也就會有 j - L 個異 pair gaps，方法數 : C(S - j, j - L)
		
		所以會被扣掉 L 個同 pair gaps，將 cnt[i + 1] 個物品放入 k 個 gaps 會產生 cnt[i + 1] - k 個同 pair，我們可列出轉移式 dp(i, j) → dp(i + 1, j - L + cnt[i + 1] - k)
		
		> 參考自 : <https://www.cnblogs.com/jiachinzhao/p/7410938.html>
	
???+note "[2021 全國賽 pF. 挑水果](https://tioj.ck.tp.edu.tw/problems/2258)"
	一開始船上有 $c$ 個種類的水果，第 $i$ 種類有 $n_i$ 顆，依序經過 $c$ 個城市，每經過一個城市可以決定要不要把船上所有前 $i$ 種類的水果給當地盤商賣，積載每顆水果經過都市 $i$ 需要積載成本 $p_i$，把每顆水果給都市 $i$ 的盤商賣需要成本 $s_i$，在都市 $i$ 賣種類 $j$ 的水果最後只會賣出 $r_{i,j}$ 顆，問若積載成本和銷售成本總和不超過 $T$ 的前提下，最多能賣幾顆水果 ?
	
	$1\le c,n_i\le 40,1\le p_i, s_i\le 1000,T \le 10^7$
	
	??? note "思路"
		類似超大背包的想法，令 dp(i, j, v): 到了第 i 個城市，上一部賣完第 j 種水果，利潤為 v 的最小成本
		
		轉移式如下，從 dp(i, j, v) 轉移過去
		
		- dp(i + 1, i + 1, v + rsum(j + 1, i + 1)) = min(dp(i, j, v) + p_sum(j + 1, n) + s_sum(j + 1, i + 1) )

		- dp(i + 1, j, v) = min(dp(i, j, v) + p_sum(j + 1, n))

		複雜度 : 狀態 O(40 * 40 * (40 * 40))，轉移 O(1)
	
???+note "[2021 全國賽 pH. 天竺鼠遊行](https://tioj.ck.tp.edu.tw/problems/2258)"
	wiwiho 校陪簡報 歷屆試題精選