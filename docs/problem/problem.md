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

???+note "[2016 全國賽 p3. 框架區間](https://tioj.ck.tp.edu.tw/problems/1913)"
	給一個 $1 \ldots n$ 的 permutation $p$，問有幾個 $(i,j)$ 滿足 $i$ 在 $p$ 內與 $j$ 在 $p$ 內的位置所形成的區間內，數字集合恰好是 $\{ i,\ldots ,j \}$
	
	$n\le 5000$
	
	??? note "思路"
		枚舉 i, j，看 pos[i], ..., pos[j] 的 min 是否為 pos[i] 且 max 是否為 pos[j]

???+note "[2022 全國賽 pD. 文字編輯器 (editor)](https://nhspc2022.twpca.org/release/problems/problems.pdf#page=11)"
	有一個由 $\texttt{+}, \texttt{[}, \texttt{]}, \texttt{x}$ 組成合法序列，此時將其中一個 $\texttt{+}$ 改成 $\texttt{|}$，並將所有 $\texttt{[}, \texttt{]}$ 換成 $\texttt{|}$。給你這個改完的序列 $s$，輸出任意一個原來的合法序列。
	
	$|s| \le 10^6$
	
	??? note "思路"
		兩個 $\texttt{x}$ 中一定要有 $\texttt{+}$，看哪兩個 $\texttt{x}$ 之間沒有 $\texttt{+}$，Greedy 的放即可

???+note "[全國賽模擬賽 2022 pI. 子集合和 (SOS)](https://www.csie.ntu.edu.tw/~b11902109/PreNHSPC2022/IqwxCSqc_Pre_NHSPC_zh_TW.pdf#page=25)"
	令函數 $f(S)=S\times \prod\limits_{x\in S}x$，問 $\sum\limits_{S\subseteq A} f(S)$
	
	$1\le n\le 10^6, 1\le a_i\le 10^9$
	
	??? note "思路"
	
		令 $G(S)=\prod \limits_{x\in S} x,\space F(S) =|S| \times \prod \limits_{x\in S} x$
	
	    則
	
	    $\begin{align}F(S \cup \{t \}) &= (|S|+1)\times \left(\prod \limits_{x\in S} x \right)\times t \\ &= F(S) \times t+G(S)\times t\end{align}$
	
	    $G(S \cup \{ t\})=G(S)\times t$
	
	    假設我們已知 $\sum \limits_{S \subseteq A}F(S)$ 和 $\sum \limits_{S \subseteq A}G(S)$，則我們可將新的 $F=$ 沒有 $t$ + 有 $t$ 
	
	    $\begin{align}\sum \limits_{S \subseteq (A + \{t \})}F(S) &= \sum \limits_{S \subseteq A}F(S)+\sum \limits_{S \subseteq A}F(S+\{ t \}) \\ &= \sum \limits_{S \subseteq A}F(S)+\left(\sum \limits_{S \subseteq A}F(S) \right)\times t +  \left(\sum \limits_{S \subseteq A}G(S) \right)\times t\end{align}$
	
	    $\sum \limits_{S \subseteq (A + \{t \})}G(S)=\sum \limits_{S \subseteq A}G(S)+\left(\sum \limits_{S \subseteq A}G(S) \right)\times t$
	    
	    ---
	    
	    > 參考自 : <https://hackmd.io/@victor26/Bkc_YXpdo>
	    
	    觀察到可能跟 $(a_1 + 1)(a_2 + 1)(a_3 + 1) \ldots (a_n + 1)$ 有關
		
		答案為 
		
		$$a_1(a_2 + 1)(a_3 + 1) \ldots (a_n + 1)+a_2(a_1 + 1)(a_3 + 1) \ldots (a_n + 1) + a_n(a_1 + 1)(a_2 + 1) \ldots (a_{n-1} + 1)$$
		
		預處理 $(a_1+1)(a_2+1)(a_3+1)...(a_n+1)$ 即可

???+note "[CF 1886 D. Monocarp and the Set](https://codeforces.com/contest/1886/problem/D)"
	問符合條件的 $1\ldots n$ 的 permutation $p$ 有幾個。給一個長度為 $(n-1)$ 的序列 $s$，$s_i$ 的意義如下 :
	
	- 若 $s_i=$ `>`，則 $p_i$ 是前綴的 max
	
	- 若 $s_i=$ `<`，則 $p_i$ 是前綴的 min
	
	- 若 $s_i=$ `?`，則 $p_i$ 兩者皆不是
	
	現在有 $q$ 筆對 $s$ 的單點修改，每次修改完輸出答案是多少
	
	$2\le n\le 3\times 10^5, 1\le m\le 3\times 10^5$
	
	??? note "思路"
		考慮列出 $p_0,\ldots ,p_{n-1}$ 的大小關係，例如 $[2,1,5,3,4]$ 的大小關係為 $p_1<p_0<p_3<p_4<p_2$。
	
	    - 若當前加入一個 `>` ，他只能放在大小關係的最後面。
	
	    - 若當前加入一個 `<`，他只能放在大小關係的最前面。
	
	    - 若當前加入一個 `?`，他可以放在大小關係的中間。
	
	    所以實際上整體的變動是由 `?` 決定的，因此，總可能性是所有 $(i-1)$ 的乘積，其中 $s_i$ == `?`（1-based）
	
	    舉例來說，$s=$ `<?>?`
	
	    - $s_1=$ `<` 則 $p_1<p_0$
	
	    - $s_2=$ `?` 則 $p_2$ 只能插入在 $p_1,p_0$ 之間，所以 $p_1<p_2<p_0$
	
	    - $s_3=$ `>` 則 $p_1<p_2<p_0<p_3$
	
	    - $s_4=$ `?` 則 $p_4$ 可以插入在中間 $3$ 個空隙中
	
	    所以答案為 $(2-1)\times (4-1)=3$

???+note "[2021 附中模競 III pF. 歡樂耶誕城 (Christmas)](https://codeforces.com/group/3Xn3T5DO0a/contest/375522/problem/F)"
	有 $n$ 條燈飾，一開始上面都沒燈泡，燈泡有 $m$ 種顏色，有以下 $q$ 次操作:
	
	1. 將指定顏色的燈泡加到指定燈飾的尾端
	
	2. 移除某條燈飾的最後一個燈泡
	
	3. 把某條燈飾變成跟另一條一模一樣
	
	4. 問某條燈飾的某個燈泡是什麼顏色
	
	$n,q\le 2\times 10^5, m\le 10^9$
	
	??? note "思路"
		把某條燈飾複製後，各自可以再延伸 ⇒ 類似 Tree 的結構
		
		用 1. 2. 3. 操作讀進來後建立完 tree 後，建 lca，對於 4. O(log n) 回答
		
???+note "[CF 1644 E. Expand the Path](https://codeforces.com/problemset/problem/1644/E)"
	有一個 n * n 的 Grid，給一個字串 s，包含 D, R，代表當前行走的路徑。可任意次的將 s 的某一項複製，但 s 不能超界，問能走到的 distinct 格子數量
	
	$n\le 10^8, |s| \le 2\times 10^5$
	
	??? note "思路"
		【套路】: 對於每一個 s 的走過的位置，計算對全局的貢獻
		
		首先，我們按照給定的操作序列執行，到達目標格子 (x, y)。然後，計算在該位置最多可以向橫向或縱向移動多少格，以達到目標位置 (n, m)。設需要向右移動 x 格，向下移動 y 格。

        在接下來的分析中，為方便起見，我們將格子與格子之間的操作轉化為格子內部的操作。例如，如果當前在 (1, 1)，需要向右移動，則我們將該格子標記為右移。當然，這樣做可能會漏掉一個格子，但後面會進行補充。

        考慮計算答案。對於每個格子，我們最多可以向相對方向（例如，如果格子是向右的，則考慮向下移動）移動 x 或 y 格。由於移動一定是向右或向下的，擴展過程不會超出邊界或與其他格子重疊，因此我們可以直接將這些貢獻添加到答案中。

        需要注意的是，只有在第一次轉向及以後才能執行這樣的操作（因為沒有辦法複製另一種方向的操作）。

        然後，考慮最後一個格子。不難發現，從右下角開始的 n * m 個格子都可以到達，我們直接將其添加到答案中。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        using namespace std;

        const int N = 2e5 + 5;
        char s[N];
        int n, ans;

        void work() {
            bool flag = 1;
            cin >> n >> s + 1;
            int x = 1, y = 1;
            for (int i = 1; s[i]; i++) {
                if (s[i] == 'R') {
                    x++;
                } else {
                    y++;
                }
                if (s[i] != s[i - 1] && i != 1) flag = 0;
            }
            if (flag == 1) {
                cout << n << '\n';
                return;
            }
            x = n - x, y = n - y;
            int i = 2;
            ans = 1;
            while (s[i] == s[i - 1]) {
                ans++;
                i++;
            }
            for (i; s[i]; i++) {
                ans++;
                if (s[i] == 'R') {
                    ans += y;
                } else {
                    ans += x;
                }
                if (s[i + 1] == 0) ans += (x + 1) * (y + 1);
            }
            cout << ans << '\n';
        }

        int main() {
            int t;
            cin >> t;
            while (t--) work();
        }
        ```
        
???+note "[CF 1644 D. Cross Coloring](https://codeforces.com/contest/1644/problem/D)"
	給一個 n * m 的 grid，每格最初都是白色的。有 q 筆操作:
	
	- color$(x_i, y_i):$ 選擇 k 種非白色的顏色的其中一種，然後將 row $x_i$ 與 $y_i$ 塗色
	
	問整張 grid 有幾種塗色方案數
	
	$n,m,k,q\le 2\times 10^5$
	
	??? note "思路"
		如果倒著考慮，題目就變成: 每次選一行一列，然後染成一個顏色，後染的色不會覆蓋原來染得顏色。
		
		那麼當一次操作會沒有貢獻，當且僅當 row 跟 column 都被完全覆蓋，否則，答案就需要乘上 k。
		
		同時我們還需要考慮一種情況，當有 n 行全部被覆蓋時，實際上相當於 m 列全部被覆蓋了；反之亦然。此後的所有操作都將變為無效操作。
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long

        using namespace std;

        const int MAXN = 2e5 + 5;
        const int mod = 998244353;
        bool row[MAXN], col[MAXN];
        int x[MAXN], y[MAXN];

        int fpow(int a, int b) {
            int res = 1;
            while (b) {
                if (b & 1) res = res * a % mod;
                a = a * a % mod;
                b >>= 1;
            }
            return res;
        }

        int main() {
            int t;
            cin >> t;
            while (t--) {
                memset(row, 0, sizeof(row));
                memset(col, 0, sizeof(col));
                int n, m, k, q;
                cin >> n >> m >> k >> q;
                for (int i = 1; i <= q; i++) {
                    cin >> x[i] >> y[i];
                }
                int cnt = 0, crow = 0, ccol = 0;
                for (int i = q; i >= 1; i--) {
                    bool ok = 0;
                    if (crow < n && ccol < m && !row[x[i]]) {
                        row[x[i]] = 1, crow++, ok = 1;
                    }
                    if (crow < n && ccol < m && !col[y[i]]) {
                        col[y[i]] = 1, ccol++, ok = 1;
                    }
                    if (ok) cnt++;
                }
                cout << fpow(k, cnt) << '\n';
            }
        }
        ```