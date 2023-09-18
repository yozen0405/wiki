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

???+note "[CS Academy - Count Arrays](https://csacademy.com/contest/archive/task/count-arrays)"
	有一個 01 序列 $a$，給 $q$ 筆限制，每筆限制 $[l_i,r_i]$ 代表在這個區間內至少要有一個 0，問 $a$ 有幾種
	
	$n,q\le 10^5$
	
	??? note "思路"
		令 dp[i] = 只考慮 1~i，$a_i$ 放 0 的合法序列有幾種，轉移的話我們就需要去枚舉放 1 的區間 $[j+1, i-1]$，所以可列出轉移式 $dp[i] = \sum dp[j]$。我們可以按照題目給的限制來預處理出 i 之前至少到哪裡都可以放 1（也就是我們 j 最小能多小），對於轉移當 i 遞增的時候，j 的最小值是單調不降的，所以可以通過這個單調性做到 $O(n)$。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    const int N = 1e5 + 5;
	    const int M = 1e9 + 7;
	
	    int n, m;
	    int mx[N], dp[N];
	
	    signed main() {
	        cin >> n >> m;
	        for (int i = 1; i <= m; i++) {
	            int l, r;
	            cin >> l >> r;
	            mx[r + 1] = max(mx[r + 1], l);
	        }
	        dp[0] = 1;
	        int now = 0, cnt = 1;
	        for (int i = 1; i <= n + 1; i++) {
	            while(now < mx[i]) {
	                cnt = (cnt - dp[now] + M) % M;
	                now++;
	            }
	            dp[i] = cnt;
	            cnt = (cnt + dp[i]) % M;
	        }
	        cout << dp[n + 1] << '\n';
	    } 
	    ```

???+note "[Zerojudge e900. 交換紙牌遊戲](https://zerojudge.tw/ShowProblem?problemid=e900)"
	共有 $n$ 個 pair，可以交換同個 pair 的兩項，目標使第一項總和與第二項總和的差值最小，問最少交換次數。
	
	$n\le 1000,1\le$ pair 中的元素 $\le 13$
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    using namespace std;
	
	    int n,a[100000],b[100000],sum,times,mi,dp[1001][13001];
	    const int INF=0x3f3f3f3f;
	
	    signed main(){
	        while(cin>>n){
	            times=INF;
	            mi=INF;
	            sum=0;
	            memset(dp,INF,sizeof(dp));
	            dp[0][0]=0;
	            //dp(i, j) 表示前 i 項，陣列 A 的總和是 j，最少要換幾次
	            for(int i=1;i<=n;i++){
	                cin>>a[i]>>b[i];
	                sum+=a[i]+b[i];
	                for(int j=1*i;j<=13*i;j++){
	                    if(j-a[i]>=0) dp[i][j]=dp[i-1][j-a[i]];
	                    if(j-b[i]>=0) dp[i][j]=min(dp[i][j],dp[i-1][j-b[i]]+1);
	                }
	            }
	            //找AB最小差值
	            for(int j=1*n;j<=13*n;j++){
	                if(dp[n][j]<INF){
	                    //abs(B的卡牌總和-A的卡牌總和)=abs((全-A)-A)=abs(sum-j-j)
	                    if(abs(sum-j-j)<mi){
	                        mi=abs(sum-j-j);
	                        times=dp[n][j];
	                    }
	                    else if(abs(sum-j-j)==mi){
	                        if(times>dp[n][j]){
	                            times=dp[n][j];
	                        }
	                    }
	                }
	
	            }   
	            cout<<times<<"\n";
	
	        }
	    }
		```

???+note "[CF 510 D. Fox And Jumping](https://codeforces.com/problemset/problem/510/D)"
	給你 $n$ 張卡片，初始坐標為 0，每張卡片都有一個 $l_i,c_i$，代表買了之後可以跳 $-l_i$ 或 $+l_i$。問可以跳到任意格子的最小花費。
	
	$n\le 300, 1\le l_i \le 10^9, 1\le c_i \le 10^5$
	
	??? note "思路"
		根據貝祖定理，$ax+by=m$ 有解 iff $m$ 為 $\gcd (a,b)$ 的倍數，反過來看，$a,b$ 的 $\gcd$ 要是 $m$ 的因數才有解
	
		同理，設當前選取卡片能跳的距離為 $a_1, \ldots ,a_k$，我們可以列出
		
		$$
		b_1 \times a_1 + b_2 \times a_2 + \ldots + b_k \times a_k = 1
		$$
		
		也就是 $\gcd(a_1, \ldots ,a_k)$ 要等於 $1$。令 $dp[i]$ 為當前能得到的gcd 等於 $i$ 的最小花費，每次用一張卡片的值去更新這些 gcd 即可。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	    #define ll long long
	
	    struct node {
	        int l, c;
	        friend bool operator<(node a, node b) {
	            return a.c > b.c;
	        }
	    } a[310];
	    map<int, int> dp;
	
	    int main() {
	        map<int, int>::iterator iter;
	        int n;
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i].l;
	        }
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i].c;
	        }
	        ll ans = 1e18;
	        sort(a + 1, a + 1 + n);
	        int gcd = a[1].l;
	        for (int i = 1; i <= n; i++) {
	            gcd = __gcd(gcd, a[i].l);
	        }
	        if (gcd > 1) {
	            cout << -1 << endl;
	            return 0;
	        }
	        for (int i = 1; i <= n; i++) {
	            dp[a[i].l] = a[i].c;
	        }
	        for (int i = 1; i <= n; i++) {
	            for (iter = dp.begin(); iter != dp.end(); iter++) {
	                if (dp[__gcd(a[i].l, iter->first)] == 0) {
	                    dp[__gcd(a[i].l, iter->first)] = a[i].c + iter->second;
	                } else {
	                    dp[__gcd(a[i].l, iter->first)] = min(dp[__gcd(a[i].l, iter->first)], a[i].c + iter->second);
	                }
	
	            }
	        }
	        cout << dp[1] << endl;
	    }
	    ```

???+note "[CF 1442 D. sum](https://codeforces.com/contest/1442/problem/D)"
	給定 $n$ 個單調不降的序列，可以從這些序列的最左端依次往右取，問取 $k$ 個數的最大值
	
	$n,k\le 3000, 0\le a_{i,j}\le 10^8, \sum |a_i| \le 10^6$
	
	??? note "思路"
		有一個序列的取部分元素，其他序列要馬全取，要馬全不取。因為若部分取兩個序列，一定可以專注取其中一個一定會更好。所以問題就轉換成 01 背包了，我們可以去枚舉取一部分的，剩下做 01 背包，但這樣會 TLE。考慮分治，當遞迴到每個 leaf 就是不包含該項的 01 背包，複雜度 $O(nk \log n)$。 
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	    #define mk make_pair
	    #define pb push_back
	    using namespace std;
	
	    const int maxn = 3e3 + 5;
	    const long long mod = 1e9 + 7;
	    int n, K, ans;
	    int a[maxn][maxn];
	    int t[maxn];
	    int tot[maxn];
	    int dp[maxn];
	
	    void solve (int l, int r) {
	        if (l > r) return;
	        if (l == r) {
	            int cur = 0;
	            for (int i = 0; i <= t[l]; i++) {
	                cur += a[l][i];
	                ans = max(dp[K - i] + cur, ans);
	            }
	            return;
	        }
	        int mid = (l + r) >> 1;
	        vector<int> tmp(K + 1);
	        for (int i = 1; i <= K; i++) {
	            tmp[i] = dp[i];
	        } 
	        for (int i = mid + 1; i <= r; i++) {
	            for (int j = K; j >= t[i]; j--) {
	                dp[j] = max(dp[j], dp[j - t[i]] + tot[i]);
	            }
	        }
	        solve (l, mid);
	        for (int i = 1; i <= K; i++) dp[i] = tmp[i];
	        for (int i = l; i <= mid; i++) {
	            for (int j = K; j >= t[i]; j--) {
	                dp[j] = max(dp[j], dp[j - t[i]] + tot[i]);
	            }
	        }
	        solve (mid + 1, r);
	    }
	
	    signed main () {
	       ios::sync_with_stdio(0);
	        cin.tie(0);
	        cin >> n >> K;
	        for (int i = 1; i <= n; i++) {
	            cin >> t[i];
	            int x;
	            for (int j = 1; j <= t[i]; j++) {
	                if(j <= K) cin >> a[i][j];
	                else cin >> x;
	                if (j <= K) tot[i] += a[i][j];
	            }
	            if (t[i] > K) t[i] = K;
	        }
	        solve (1, n);
	        cout << ans;
	    }
	    ```

???+note "[CF 1483 C. Skyline Photo](https://codeforces.com/problemset/problem/1483/C)"
	給 $n$ 個建築，每個建築有高度 $a_i$ 和美麗值 $b_i$。劃分成若干個連續段，使得所有區間的貢獻之和最大。其中每個區間的貢獻值為，區間中高度最低的建築物的美麗值。輸出最大貢獻和。
	
	$n\le 3\times 10^5, 0\le |b_i| \le 10^9$
	
	??? note "思路"
		$$dp(i)=\max \{dp_j + \text{cost}(j + 1, i) \}$$
		
		我們想辦法利用線段樹來快速查詢最大值，但瓶頸在於後面的 cost 沒辦法很快地計算。不過可以觀察到實際上在 cost 貢獻的那一項可以用一個單調隊列（遞增）維護，複雜度 $O(n \log n)$
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pb push_back
	    #define mk make_pair
	    #define pii pair<int, int>
	    using namespace std;
	
	    const int INF = 9e18;
	    const int maxn = 3e5 + 5;
	    int a[maxn], w[maxn], stk[maxn], dp[maxn];
	    int n;
	
	    struct seg {
	        int mx, tag;
	        seg *lch, *rch;
	        seg () {
	            tag = 0;
	            mx = 0;
	            lch = rch = nullptr;
	        }
	        void push () {
	            if (tag) {
	                lch -> mx += tag;
	                rch -> mx += tag;
	                lch -> tag = tag;
	                rch -> tag = tag;
	                tag = 0;
	            }
	        }
	        void modify (int l, int r, int mL, int mR, int val) {
	            if (mL <= l && r <= mR) {
	                mx += val, tag += val;
	                return;
	            }
	            int mid = (l + r) >> 1;
	            if (!lch) lch = new seg();
	            if (!rch) rch = new seg();
	            push();
	            if (mL <= mid) {
	                lch -> modify(l, mid, mL, mR, val);
	            } 
	            if (mid + 1 <= mR) {
	                rch -> modify(mid + 1, r, mL, mR, val);
	            } 
	            mx = max(lch -> mx, rch -> mx);
	        }
	        int query (int l, int r, int qL, int qR) {
	            if (qL <= l && r <= qR) {
	                return mx;
	            }
	            int mid = (l + r) >> 1;
	            if (!lch) lch = new seg();
	            if (!rch) rch = new seg();
	            push();
	            int ret = -INF;
	            if (qL <= mid) {
	                ret = max(ret, lch -> query(l, mid, qL, qR));
	            } 
	            if (mid + 1 <= qR) {
	                ret = max(ret, rch -> query(mid + 1, r, qL, qR));
	            } 
	            return ret;
	        }
	    };
	
	    void init () {
	        cin >> n;
	        for (int i = 1; i <= n; i++) {
	            cin >> a[i];
	        }
	        for (int i = 1; i <= n; i++) {
	            cin >> w[i];
	        }
	    }
	
	    void solve () {
	        seg *rt = new seg();
	        a[0] = -INF;
	        int top = 1;
	        for (int i = 1; i <= n; i++) {
	            while (top && a[stk[top - 1]] >= a[i]) {
	                rt -> modify(0, n, stk[top - 2], stk[top - 1] - 1, -w[stk[top - 1]]);
	                top--;
	            }
	            rt -> modify(0, n, stk[top - 1], i - 1, w[i]);
	            dp[i] = rt -> query(0, n, 0, i - 1);
	            rt -> modify(0, n, i, i, dp[i]);
	            stk[top++] = i;
	        }
	        cout << dp[n] << "\n";
	    }
	
	    signed main () {
	        init();
	        solve();
	    }
	    ```
	    
???+note "[2019 全國賽 pG. 隔離採礦](https://judge.tcirc.tw/ShowProblem?problemid=d088)"	
	有 $n$ 個礦井，每個礦井有高度 $h_i$ 與價值 $v_i$，挑一些礦井，使得相鄰兩個礦井間都存在一個更高的礦井
	
	$n\le 10^6, h_i\le 10^9, v_i\le 10^5$
	
	??? note "思路"
		$$dp[i]=\max \{ dp[j]+v[i] \}$$
		
		可以發現能轉移的 $j$ 會長這樣 :
		
		<figure markdown>
          ![Image title](./images/1.png){ width="300" }
          <figcaption>綠色有辦法轉移，紅色沒辦法</figcaption>
        </figure>
        
        所以我們可以用單調 stack 維護無法轉移的 $j$。但不能直接取 stack.top，因為可能會發生 $h_i=h_j$ 的情況，所以我們必須在 stack 內二分出比 $h_i$ 大且最靠近 $i$ 的 $j$，用 BIT 去 query_max$(1, j)$ 來轉移即可
        
    ??? note "code"
    	```cpp linenums="1"
    	#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        const int N = 1e6 + 5;
        int n;
        int h[N], v[N], dp[N];

        struct BIT {
            #define lowbit(x) (x & (-x))
            int n;
            vector<int> bit;

            BIT(int _n) {
                n = _n;
                bit = vector<int>(n + 1, 0);
            }
            int query(int x) {
                int ret = 0;
                while (x > 0) {
                    ret = max(ret, bit[x]);
                    x -= lowbit(x);
                }
                return ret;
            }
            void update(int x, int d) {
                while (x <= n) {
                    bit[x] = max(bit[x], d);
                    x += lowbit(x);
                }
            }
        }; 

        signed main() {
            cin >> n;
            for (int i = 1; i <= n; i++) {
                cin >> h[i];
            }
            for (int i = 1; i <= n; i++) {
                cin >> v[i];
            }
            BIT bit(n);
            vector<int> stk;
            for (int i = 1; i <= n; i++) {
                while (stk.size() && h[stk.back()] < h[i]) {
                    bit.update(stk.back(), dp[stk.back()]);
                    stk.pop_back();
                }
                int l = 0, r = stk.size();
                while (r - l > 1) {
                    int mid = (l + r) / 2;
                    if (h[stk[mid]] <= h[i]) {
                        r = mid;
                    } else {
                        l = mid;
                    }
                }

                if (stk.size() && h[stk[l]] > h[i]) {
                    dp[i] = bit.query(stk[l]) + v[i];
                } else {
                    dp[i] = v[i];
                }
                stk.push_back(i);
            }
            cout << *max_element(dp + 1, dp + n + 1) << '\n';
        }
    	```
    	
???+note "[洛谷 P4141 消失之物](https://www.luogu.com.cn/problem/P4141)"
    有 $n$ 個物品，體積分別是 $w_1,w_2,\dots,w_n$。第 $i$ 個物品丟失了。

    「要使用剩下的 $n-1$ 物品裝滿容積為 $x$ 的背包，有幾種方法呢 ?」

    把答案記為$\text{cnt}(i,x)$ ，輸出所有$i \in [1,n]$, $x \in [1,m]$ 的$\text{cnt}(i, x)$。
    
???+note "[CF 730 J. Bottles](https://codeforces.com/problemset/problem/730/J)"
	有 $n$ 個瓶子，各有水量 $a_i$ 和容量 $b_i$。現在要將這寫瓶子裡的水存入最少的瓶子裡。問最少需要的瓶子數，與在保證瓶子數最少的情況下，轉移的水量最少是多少。
	
	$n\le 100, 1\le a_i,b_i\le 100$
	
	??? note "思路"
		首先，最少瓶子數可以透過貪心的枚舉前幾大的 $b_i$，看枚舉到哪時可以裝得下 $\sum a_i$。
		
		在來，我們來整理一下「最少轉移的水量」所需符合的條件 :
		
		- 需要 ans1 個來儲存（第一個答案）

		- 容量和要足夠讓剩下的水量倒進去

		- 轉移的水量越少越好 ⇒ 已固定的水量要越大越好

		考慮 dp(i, j, k) = 考慮 1...i，我們已經選了 j 個來儲存，容量能湊到 k 的，水量最多可以是多少
        
        最後的答案就是 dp(n, ans1, $\sum a_i \ldots \sum b_i$) 取 max
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pb push_back
        #define mk make_pair
        #define a first
        #define b second
        #define pii pair<int, int>
        using namespace std;

        const int INF = 9e18;
        const int maxn = 105;
        int n, m;
        int sumA, sumB, dp[maxn][maxn * maxn];
        vector<pii> v;

        void solve () {
             cin >> n;
             v.resize(n + 1);
             for (int i = 1; i <= n; i++) {
                  cin >> v[i].a;
                  sumA += v[i].a;
             }
             for (int i = 1; i <= n; i++) {
                  cin >> v[i].b;
                  sumB += v[i].b;
             }

             sort (v.begin() + 1, v.end(), [](pii x, pii y) { return x.b > y.b; });
             int N;
             for (int i = 1, s = 0; i <= n; i++) {
                  s += v[i].b;
                  if (s >= sumA) {
                       N = i;
                       break;
                  }
             }

             // dp[i][j] = 容量為 j 的最大水量
             memset (dp, -1, sizeof dp);
             dp[0][0] = 0;
             for (int i = 1; i <= n; i++) {
                  for (int j = sumB; j >= v[i].b; j--) {
                       for (int k = i; k >= 1; k--) {
                            if (dp[k - 1][j - v[i].b] != -1)
                                 dp[k][j] = max(dp[k - 1][j - v[i].b] + v[i].a, dp[k][j]);
                       }
                  }
             }

             int ans = 0;
             for (int j = sumA; j <= sumB; j++) {
                  ans = max(ans, dp[N][j]);
             }
             cout << N << " " << sumA - ans << "\n";
        }

        signed main () {
            solve();
        }
        ```
        
???+note "[CF 366 C. Dima and Salad](https://codeforces.com/problemset/problem/366/C)"
	有 $n$ 個物品，每個物品有權值 $a_i$ 與 $b_i$，選一些物品，使得
	
	$$
	\frac{\sum \limits_{j=1}^m a_j}{\sum \limits_{j=1}^m b_j} =k
	$$
	
	滿足上面條件的這一些物品 $a_i$ 和最大可以是多少
	
	$n\le 100, 1\le k\le 10, 1\le a_i, b_i\le 100$
	
	??? note "思路"
		類似 01 分數規劃的形式，問題就變成 :
		
		有 $n$ 個物品重量 $a_i - k\times b_i$ 與價值 $a_i$，求重量為 $0$ 最大的 $a_i$ 總和
		
		實作上不能狀態壓縮，因為 $a_i - k\times b_i$ 有可能是正的也有可能是負的，可以用滾動陣列代替。本來重量和的 range 會在 [-1e5, 1e5]，但陣列 index 不能用負的，所以要改成 [0, 2e5]，最後的答案就是 dp(n, 1e5)
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>

        using namespace std;

        const int N = 105;
        const int M = 1e5;
        int n, k;
        int a[N], b[N], dp[2][200005];

        signed main() {
            cin >> n >> k;
            for (int i = 1; i <= n; i++) {
                cin >> a[i];
            }
            for (int i = 1; i <= n; i++) {
                cin >> b[i];
            }
            memset(dp, -0x3f, sizeof(dp));
            dp[0][M] = 0;
            for (int i = 1; i <= n; i++) {
                int w = a[i] - k * b[i];
                for (int j = 2e5; j >= 0; j--) {
                    if (j - w >= 0 && j - w <= 2e5) {
                        dp[i % 2][j] = max(dp[(i - 1) % 2][j], dp[(i - 1) % 2][j - w] + a[i]);
                    }
                }
            }
            if (dp[n % 2][M] == 0) {
                cout << "-1\n";
            } else {
                cout << dp[n % 2][M] << '\n';
            }
        } 
        ```
	
	