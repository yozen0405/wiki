## 例題

### Maximum AND

???+note "[CF 1721 D. ](https://codeforces.com/problemset/problem/1721/D)"

	??? note "思路"
		D&C

### Maximum Xor

???+note "[CSES - ](https://cses.fi/problemset/task/1655)"

### Bit Problem

???+note "[CSES ](https://cses.fi/problemset/task/1654)"

	??? note "思路"
		sos dp

???+note "[2019-pre-pC](https://drive.google.com/file/d/1ECLkM85zf-TS8wMCWiqQP89PNK_b6JZQ/view)"
	
???+note "[Xor tree](https://codeforces.com/problemset/problem/1709/E)"

???+note "[Bits And Pieces](https://codeforces.com/problemset/problem/1208/F)"

???+note "[Xor Minimization](https://atcoder.jp/contests/abc281/tasks/abc281_f)"

???+note "[CF 1847 F. The Boss's Identity](https://codeforces.com/contest/1847/problem/F)"

???+note "[CF 1851 F. Lisa and the Martians](https://codeforces.com/contest/1851/problem/F)"
	
### IOIC 

???+note "<a href="/wiki/other/images/ioic_201.html" target="_blank">2023 IOIC 201 . 獨一無二的區間和(ㄏㄢˋ)</a>"
	
	??? note "思路"
		估計：證明 $n > 30$ 時無解
		
		假設有解，令 $b_i = a_1 \, \& \, a_2 \, \& \, \cdots \, a_i$。
		
		1. 根據定義，$b_1, b_2, \ldots, b_{31}$ 必須兩兩不同。
	    
	    2. 因為 $b_{i + 1} = b_i \, \& \, a_{i + 1}，b_{i + 1}$ 是 $b_i$ 的 submask。

	    綜上兩點，$b_{i + 1}$ 是 $b_i$ 的 proper submask。這使得
		
		```
	    __builtin_popcount(b[1]) > __builtin_popcount(b[2]) > ... > __builtin_popcount(b[31])
	    ```
	    
	    因為 $0 \leq b_1, b_2, \ldots, b_{31} < 2^{30}$，__builtin_popcount 的值域是 $[0, 30]$
	    $\Rightarrow \,$ `__builtin_popcount(b[i])` = $31 - i$
	    $\Rightarrow \, b_1 = a_1 = 111111111111111111111111111111_2$
	    $\Rightarrow \, a_1 \, \& \, a_2 = a_2 \quad \rightarrow\leftarrow$
	
	    構造：證明 $n \leq 30$ 時總是有解
	    
	    $a_i = 2^{30} - 1 - 2^{i}$ 是個合法構造。

???+note "<a ref="/wiki/other/images/ioic_301.html" target="_blank">2023 IOIC 301 . SOS</a>"

	??? note "思路"
		> SOS
		
        為了解釋方便，我們把 $AND$ 符號改成 $\subseteq$，並且會用二進位數字表示集合。

        令 $N = \lceil \log_2{n+1} \rceil$，也就是 $n$ 的二進位表示中的位數。整題的 $N \leq 20$

        > Subtask 1 ($N \leq 10$)
        
        暴力枚舉，$O(3^N)$ 或 $O(4^N)$ 都可以過。

        > Subtask 2 滿分解
        
        令 $g(x) = \bigoplus _ {0 \leq y \ , \ x \subseteq y = y} f(y)$
        則，$f(x) = a_x + \sum _ {0 \leq y < x \ , \ x \subseteq \ y = y} g(y)$

        令 $F[i][j]$ 代表和 $i$ 的前 $n - j$ 個位元相同的所有子集 $y$ 的 $g(y)$ 的和。

        同理，$G[i][j]$ 代表和 $i$ 的前 $n - j$ 個位元相同的所有子集 $y$ 的 $f(y)$ 的和。

        那麼可以枚舉 $i$ 由小到大，先計算所有 $F[i][*]$之後得到 $f(i) = F[i][N] + a_i$，再計算所有 $G[i][*]$。

        轉移是：
        $$
        F[i][j] = \begin{cases}
            F[i][j-1] &,i \& 2^j = 0\\
            F[i][j-1] + F[i \oplus 2^j][j-1] &,\text{otherwise}
            \end{cases}
        $$

        $$
        G[i][j] = \begin{cases}
            G[i][j-1] &,i \& 2^j = 0\\
            G[i][j-1] \oplus G[i \oplus 2^j][j-1] &,\text{otherwise}
            \end{cases}
        $$

        複雜度 $O(2^N N)$

	??? note "code"

        ```cpp linenums="1"
        //Challenge: Accepted
        #pragma GCC optimize("Ofast")
        #include <bits/stdc++.h>
        using namespace std;
        #ifdef zisk
        void debug(){cout << endl;}
        template<class T, class ... U> void debug(T a, U ... b){cout << a << " ", debug(b...);}
        template<class T> void pary(T l, T r) {
            while (l != r) cout << *l << " ", l++;
            cout << endl;
        }
        #else
        #define debug(...) 0
        #define pary(...) 0
        #endif
        #define ll long long
        #define maxn 1050005
        #define pii pair<int, int>
        #define ff first
        #define ss second
        #define io ios_base::sync_with_stdio(0);cin.tie(0);
        const ll inf = 1LL<<60;
        int fs[maxn][21], gs[maxn][21];
        ll f[maxn], g[maxn];
        const int se = (1<<30)-1;
        void madd(int &x, int y) {
            x = (x + y) & se;
        }
        int main() {
            io;
            int n;
            cin >> n;
            for (int i = 0;i < n;i++) cin >> f[i];
            for (int i = 0;i < n;i++) {
                for (int j = 0;j < 20;j++) {
                    if (j) madd(fs[i][j], fs[i][j-1]);
                    if ((i>>j)&1) madd(fs[i][j], fs[i ^ (1<<j)][j]);
                }
                f[i] += fs[i][19];
                f[i] &= se;
                gs[i][0] ^= f[i];
                for (int j = 0;j < 20;j++) {
                    if (j) gs[i][j] ^= gs[i][j-1];
                    if ((i>>j)&1) gs[i][j] ^= gs[i ^ (1<<j)][j];
                }
                g[i] = gs[i][19];
                for (int j = 0;j < 20;j++) {
                    madd(fs[i][j], g[i]);
                }
            }	
            cout << f[n-1] << "\n";
        }
        ```

???+note "<a ref="/wiki/other/images/ioic_305.html" target="_blank">2023 IOIC 305 . 括號國</a>"

	??? note "code"
		```cpp linenums="1"
		#pragma GCC optimize("O3")
        #pragma GCC target("popcnt")
        #pragma GCC target("sse,sse2,sse3,ssse3,abm,avx")
        #include <bits/stdc++.h>
        using namespace std;

        using ll = long long;

        const int N = 2e5 + 5;

        int n, r;
        ll ans;
        string s;
        int st[N];

        int main() {
            ios::sync_with_stdio(false), cin.tie(0);
            cin >> n >> s;
            s = " " + s;
            for (int i = 1; i <= n; i++) {
                if (s[i] == '(') st[++r] = i;
                else if (s[i] == ')' && r > 0) ans += 1ll * st[r--] * (n - i + 1);
            }
            cout << 2 * ans << '\n';
        }
		```
