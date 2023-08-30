## 名詞解釋

T 是 S 的子集（subset），反過來，集合 S 就是 T 的一個超集（superset）

<figure markdown>
  ![Image title](./images/11.png){ width="200" }
</figure>

## 引入

??? note "問題"
	input: 每個 $[0, 2^n-1]$ 的 subset T 都有一個 weight w(T)
	
	output: $f(S) = \sum \limits_{T 是 S 的 subset} w(T)$

### 法 1: 枚舉子集的子集

內層 popcount 為 $x$ 的 mask 需要花 $O(2^x)$，共有 $C^n_x$ 個，總計 $O\left( \sum \limits_{x=0}^n C^n_x 2^x\right)$

又根據二項式定理，$(1+a)^n=\sum \limits_{x=0}^n C^n_x a^x$，則 $O\left ( \sum \limits_{x=0}^n C^n_x 2^x = (1+2)^n=3^n\right )$

???+note "code"
	```cpp linenums="1"
	for (int mask = 0; mask < (1 << n); mask++) {
		// 給一個 mask，枚舉他的所有子集合
		for (int S = mask; S >= 0; S = (S - 1) & mask) {
			// TODO
        }
    }
    ```

### 法 2 : sos dp

dp(mask, i) : 只枚舉 mask 的 [0, i] 而其餘不動的總和

dp(S, i) : 看第 i 位選不選

- if (S & (1<<i)) dp(S, i) = dp(S, i-1) + dp(S ^ (1<<i), i-1)

- else dp(S, i) = dp(S, i-1)

???+note "code"
	```cpp linenums="1"
	for (int i = 0; i < n; i++) {
		f[a[i]]++;
	}

    for (int i = 0; i < 21; i++) {
        for (int mask = 0; mask < (1 << 21); mask++) {
            if (mask & (1 << i)) {
            	f[mask] += f[mask ^ (1 << i)];
            }
        }
    }
    ```

### 技巧 : 枚舉超集

???+note "問題"
	給一個陣列 $a_1,\ldots ,a_n$，對於每個 $i$ 問有沒有 $a_i$ & $a_j=a_i$
	
舉例來說，若 
    
- x = 1101001
- y = 11<font style="color:#00A2E8">0</font>1<font style="color:#00A2E8">0</font><font style="color:#00A2E8">0</font>1

y 只能在藍色的部分從 0 變 1，代表若 0, 1 顛倒後 y 就要是 x 的 subset

- (~x) = 0010110
- (~y) = 00<font style="color:#00A2E8">1</font>0<font style="color:#00A2E8">1</font><font style="color:#00A2E8">1</font>0

所以答案就是 subset of (~$x$) in set(~$a_i$)

為了之後方便使用，以下的 code 令 d[i] 為 x & i = i 的 x 的數量

??? note "code"
	```cpp linenums="1"
	const int C = (1LL << 21) - 1;
	
	vector<int> build(vector<int> a) {
		int n = a.size();
        vector<int> f(C + 1);
        vector<int> d(C + 1);
        for (int i = 0; i < n; i++) {
            f[C ^ a[i]]++;
        }
        for (int i = 0; i < 21; i++) {
            for (int mask = 0; mask < (1 << 21); mask++) {
                if (mask & (1 << i)) {
                    f[mask] += f[mask ^ (1 << i)];
                }
            }
        }
        for (int mask = 0; mask < (1 << 21); mask++) { 
            d[mask] = f[C ^ mask];
            // d[i] => x & i = i 的 x 數量
        }
        return d;
    }
    ```
	
## 例題

### CSES Bit Problem

???+note "[CSES - Bit Problem](https://cses.fi/problemset/task/1654)"
    對於每筆 $\text{query}(x)$ 分別計算以下的 $y$ 的數量
    
    - $x \mid y = x$
    
    - $x\mathrel{\&} y=x$
    
    - $x \mathrel{\&} y \neq 0$ 
    
    ??? note "思路"
        > subtask : x | y = x
    
        裸 sos dp
        
        > subtask : x & y = x
    
        上面的超集 sos dp
    
        > subtask : x & y ≠ 0
    
        ans = n - subset of (~x) in set(~a[i])
        
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
        const int C = (1LL << 21) - 1;

        int n;
        int F[(1LL << 21) + 2], R[(1LL << 21) + 2];
        int a[maxn];

        void build () {
            for (int i = 0; i < n; i++) F[a[i]]++;

            for (int i = 0; i < 21; i++) {
                for (int mask = 0; mask < (1 << 21); mask++) {
                    if (mask & (1 << i))
                        F[mask] += F[mask ^ (1 << i)];
                }
            }

            for (int i = 0; i < n; i++) R[C ^ a[i]]++;

            for (int i = 0; i < 21; i++) {
                for (int mask = 0; mask < (1 << 21); mask++) {
                    if (mask & (1 << i))
                        R[mask] += R[mask ^ (1 << i)];
                }
            }
        }

        void init () {
            cin >> n;
            for (int i = 0; i < n; i++) cin >> a[i];
        }

        void solve () {
            build ();
            for (int i = 0; i < n; i++) {
                int Q1 = F[a[i]];
                int Q2 = R[C ^ a[i]];
                int Q3 = n - F[C ^ a[i]];
                cout << Q1 << " " << Q2 << " " << Q3 << "\n";
            }
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
		
### CF Compatible Numbers

???+note "[CF 449 D. Jzzhu and Numbers](https://codeforces.com/contest/449/problem/D)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，問有幾個子序列滿足 $a_{i_1}$ & $a_{i_2}$ & $\ldots$ & $a_{i_k}=0$
	
	$1\le n\le 10^6,0\le a_i\le 10^6$
	
	??? note "思路"
		令 d[i] 為 x & i = i 的 x 的數量，共可湊出 2^d[i]-1 種子集。
	
		因為正著做不好做，我們考慮讓 「ans = 全部 - 至少 1 個 bit 是 1」
		
		但 SOS dp 是會重複算的，例如 11 會在 d[10], d[01] 都被算過一遍
		
		考慮排容 : ans = 全部 - 至少 1 個 bit 是 1 + 至少 2 個 bit 是 1 - 至少 3 個 bit 是 1
		
		<figure markdown>
          ![Image title](./images/12.png){ width="300" }
        </figure>
        
        所以我們只要枚舉 i = [1, C]，看 popcount(i) 是奇數還偶數，將貢獻加或減 2^d[i]-1 即可
        
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
        const int maxn = 1e6 + 5;
        const int M = 1e9 + 7;
        const int C = (1LL << 21) - 1;

        int n;
        int a[maxn];

        vector<int> build() {
            vector<int> f(C + 1);
            vector<int> d(C + 1);
            for (int i = 0; i < n; i++) {
                f[C ^ a[i]] = (f[C ^ a[i]] + 1) % M;
            }
            for (int i = 0; i < 21; i++) {
                for (int mask = 0; mask < (1 << 21); mask++) {
                    if (mask & (1 << i)) {
                        f[mask] = (f[mask] + f[mask ^ (1 << i)]) % M;
                    }
                }
            }
            for (int mask = 0; mask < (1 << 21); mask++) { 
                d[mask] = f[C ^ mask];
            }
            return d;
        }

        int fpow(int a, int b) {
            int ret = 1;
            while (b != 0) {
                if (b & 1) ret = (ret * a) % M;
                a = (a * a) % M;
                b >>= 1;
            }
            return ret;
        }

        signed main() {
            cin >> n;
            for (int i = 0; i < n; i++) {
                cin >> a[i];
            }
            vector<int> d = build();

            int ans = 0;
            for (int i = 0; i < (1 << 21); i++) {
                if (__builtin_popcount(i) & 1) {
                    ans = ((ans - (fpow(2, d[i]) - 1)) % M + M) % M;
                } else {
                    ans = ((ans + (fpow(2, d[i]) - 1)) % M + M) % M;
                }
            }
            cout << ans << '\n';
        } 
        ```
	
### CF Bits And Pieces

???+note "[CF 1208F Bits And Pieces](https://codeforces.com/problemset/problem/1208/F)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，輸出 $i<j<k$ 時最大的 $a_{i} \mid ( a_{j} \mathrel{\&} a_{k} )$ 
	
	$3\le n\le 10^6,0\le a_i\le 2\times 10^6$
	
	??? note "思路"
		d[i]: 維護最大的兩個 index
	
		對於每個 $a_i$，greedy 的從高位到低位看 d[x].S 是否大於 i
		
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
        const int maxn = 1e6 + 5;
        const int M = 1e9 + 7;
        const int C = (1LL << 21) - 1;

        int n;
        int a[maxn];

        pii sec(pii p, int x) {
            if (x > p.F) {
                p.S = p.F;
                p.F = x;
            } else if (x > p.S) {
                p.S = x;
            }
            return p;
        }

        vector<pii> build() {
            vector<pii> f(C + 1, {-1, -1});
            for (int i = 0; i < n; i++) {
                f[C ^ a[i]] = sec(f[C ^ a[i]], i);
            }
            for (int i = 0; i < 21; i++) {
                for (int mask = 0; mask < (1 << 21); mask++) {
                    if (mask & (1 << i)) {
                        f[mask] = sec(f[mask], f[mask ^ (1 << i)].F);
                        f[mask] = sec(f[mask], f[mask ^ (1 << i)].S);
                    }
                }
            }
            vector<pii> d(C + 1);
            for (int mask = 0; mask < (1 << 21); mask++) { 
                d[mask] = f[C ^ mask];
            }
            return d;
        }

        signed main() {
            cin >> n;
            for (int i = 0; i < n; i++) {
                cin >> a[i];
            }
            vector<pii> dp = build();

            int ans = 0;
            for (int i = 0; i < n - 2; i++) {
                int res = 0, ret = 0;
                for (int j = 20; j >= 0; j--) {
                    if (a[i] & (1 << j)) {
                        res |= (1 << j);
                        continue;
                    } else {
                        if (dp[ret | (1 << j)].S > i) {
                            ret |= (1 << j);
                            res |= (1 << j);
                        }
                    }
                }
                ans = max(ans, res);
            }
            cout << ans << '\n';
        } 
        ```

### JOI 2018 p5

???+note "[JOI 2018 p5](https://oj.uz/problem/view/JOI18_snake_escaping)"
	每個數自 $i$ 都有對應的 $w[i]$，給你 $Q$ 筆 $\text{query}$ 每筆會是一個 `0`, `1`, `?` 組成的二進制，`?` 可以是 `0` or `1`。問可以組出的 subset $s$ 的 $\sum w[s]$
	
	??? note "思路"
	    - 最主要的觀察是鴿籠原理，$L\le 20$ 代表 `0` `1` `?` 最少的最大出現次數是 $6$
	    - 所以看 `0` `1` `?` 哪個比較少就用哪個下手
	    
	    > calculate ?
	
	    - 暴力枚舉 $\texttt{?}$ 要是什麼
	    - $\begin{align}O(2^{\text{cnt[?]}})\end{align}$
	
	    > calculate 0
	    
	    - 容斥原理的 superset
	    - $\text{g}[10110]$ 包括 $\text{g}[11110],\text{g}[10111],\text{g}[11111]$
	    - $1001?011$
	    - 我要看的是那些 $0$，固定的要是那些 $0$
	      - 所以問號會被我設為 $0$ 因為在這個情況 $0=$ **有**
	    - 所以我需要減掉那些位置應該要是 $0$ 只是在做 subset 的時候有機會變 $1$ 的 $\texttt{bit}$
	    - 做完容斥原理後我們應該會剩下 
	      - $1001\color{red}0\color{white}11$
	      - $1001\color{red}1\color{white}11$
	    - 這代表什麼 $\texttt{?}$
	      - 我們 $\text{g[]}$ 的 $\texttt{init}$ 也就是 $\text{g[0], g[1], g[2], g[3],}\ldots$ 等於 $\text{a[0], a[1], a[2], a[3],}\ldots$
	    - 我們只是用 $0$ 來代表我們有計算到的 $\texttt{bit}$，本質是沒變的
	    - 只是針對 $0$ 來代表 **有** 的概念
	    - $O(2^\text{cnt[0]})$
	
	    ```cpp linenums="1"
	    for(int i = 0; i < L; ++i)
	        for(int j = (1 << L) - 1;j >= 0; j--)  
	            // 注意 j 從大到小 (對於 0 來說是從 null to all)
	            if(((1<<i)&j)==0) g[j] += g[j^(1<<i)];
	    ```
	
	    > calculate 1
	
	    -  容斥原理的 subset
	    - $1001?011$
	    - 我要看的是那些 $1$，固定的要是那些 $1$
	      - 所以問號會被我設為 $1$ 因為在這個情況 $1=$ **有**
	    - 所以我需要減掉那些位置應該要是 $1$ 只是在做 subset 的時候有機會變 $0$ 的 $\texttt{bit}$
	    - 做完容斥原理後我們應該會剩下 
	      - $1001\color{red}0\color{white}11$
	      - $1001\color{red}1\color{white}11$
	    - $O(2^\text{cnt[1]})$
	
	    ```cpp linenums="1"
	    for(int i = 0; i < L; ++i)
	        for(int j = 0;j < (1 << L); ++j)
	            if(((1 << i) & j) == 0) f[j] += f[j^(1<<i)];
	    ```
	    
	    參考自 $\texttt{:}$ https://www.cnblogs.com/nightsky05/p/15563014.html


---

## 資料

- [CF 詳細講解](https://codeforces.com/blog/entry/45223)

- [cnblog SOS DP 学习笔记](https://www.cnblogs.com/cyl06/p/SOSDP.html)