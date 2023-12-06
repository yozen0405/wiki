# 括號問題

## 解法一覽

- 一、卡特蘭數

- 二、stack 解法

- 三、區間 dp（適用於有很多種括號類型時）

- 四、前綴 dp（適用於只有一種括號類型）

- 五、左括號想成 +1，右括號想成 -1，min prefix sum >= 0，sum = 0
	
	- greedy 修改
	
	- 資料結構優化

## 左括號+1，右括號-1

???+note "[2023 全國賽模擬賽 pF. 關卡設計 (level)](https://codeforces.com/gym/104830/attachments/download/23302/zh_TW.pdf#page=17)"
    給一個括號序列，一次操作可將一個括號改變方向，問是否能恰好做 k 次操作使括號序列合法，若可以的話輸出一組解
    
    $n, k \le 2 \times 10^6$

	??? note "思路"
		**【一、轉換問題】**
	
	    如果將左括號想成 +1，右括號想成 -1， 那麼合法的條件是
	
	    1. 每個 prefix sum 都大於等於 0
	    2. 總和為 0
	
	    **【二、修改的 greedy 策略】**
	
	    也就是我們需要將一些 -1 要變成 +1，一些 +1 要變成 -1。因為要盡量讓每個 prefix sum 都大於等於 0，我們得到了一個 greedy 的策略:
	
	    - -1 改成 +1 的，優先改愈左邊的愈好
	
	    - +1 改成 -1 的，優先改愈右邊的愈好
	
	    **【三、求得修改個別的數量】**
	
	    我們假設一開始有 x 個 +1, y 個 -1，-1 改成 +1 的有 u 個，+1 改成 -1 的有 v 個。那麼我們是否可以直接透過 x, y, k 得到 u, v ?
	
	    因為總共就只能改 k 個，所以 u + v = k，一開始的 sum = x - y，之後的每個 u 會讓 sum += 2，每個 v 會讓 sum -= 2。而最後的 sum 要是 0，所以  sum = (x-y) + 2u - 2v = 0，我們就可以解一元二次方程式得到 u, v，再套用步驟二的 greedy 策略即可。無解的話可以在解 u, v 或是做 greedy 完後得到。
	
		---
		
		> 另一種想法:
	
	    為了簡化題目，我們將原本的括號序列每一項都替換成左括號。接著，我們令原本的括號序列為 a，依照上一句話替換後的序列為 b，則對於修改的貢獻，我們可以這麼想: 假設有一個陣列 v，若 a[i] != b[i]，則 v[i] = -1，代表反悔操作，反則若 a[i] = b[i]，則 v[i] = 1，代表花費一單位的貢獻改變第 i 項。我們的目標就是要從 b 選出 n / 2 項，讓他們重新變回右括號，而且要滿足: 括號序列須合法、最後的序列只能是更動 k 項，那其實這就等價於在 v 內選 n/2 項，讓他們的總和是 k'（k' 就是 a 改成 b 後還能修改的次數）。我們將式子列出來，令要選的 1 的總和為 x，-1 的總合為 y，則可以列出
	
	    <div style="text-align: center;">
	      <div style="display: inline-block; text-align: left;">
	        x - y = n / 2 <br>
	        x + y = k'
	      </div>
	    </div>
	
	    舉個實際的例子，例如說 n = 10，k = 6，`a = )))))(((((`，則 `b = (((((((((( `，所以 `v = [-1, -1, -1, -1, -1, 1, 1, 1, 1, 1]`。此時 k' = 6 - 5 = 1。我們來解二元一次方程式
	
	    <div style="text-align: center;">
	      <div style="display: inline-block; text-align: left;">
	        x - y = 5 <br>
	        x + y = 1
	      </div>
	    </div>
	
	    得到 x = 3, y = -2。
	
	    x, y 若會不合法，此時可輸出無解，否則在得到 x, y 之後，我們就可以 greedy 的由後往前，將 b 變成我們最終的答案，具體來說，我們從後往前考慮，假如此時看到第 i 項，我們就看 v[i] 是對應到 x 或 y，若對應到的（假如是 x）還有剩，則就將 b[i] 改動，否則，我們就繼續往前面一項考慮。注意，最後改完的時候，也要記得檢查是否為不合法的括號序列。例如說 n = 10, k = 2, `a = )))))(((((`，則 `b = (((((((((( `，所以 `v = [-1, -1, -1, -1, -1, 1, 1, 1, 1, 1]`。此時 k' = 2 - 5 = -3。我們解二元一次方程式可得到 x = 1, y = -4，但這樣改完後續列為 `())))(((()`，不合法。

???+note "最少修改次數"
	給一個長度為 $n$ 的括號序列，一次操作可將一個括號改變方向，問至少幾次操作才可以使括號序列合法
	
	$n\le 2\times 10^6$
	
	??? note "思路"
	
	    一樣使用上面分析的觀點，假設需要 u 個 -1 變成 +1，v 個 +1 變成 -1，我們的目標是最小化 u + v。我們分成兩個步驟: 
	
	    1. 使 sum 變成 0: 依照 (x-y) + 2u - 2v = 0，我們可求得 u - v，我們先將 u, v 其中一個設成 0，使 u + v 最小
	    2. 使 min prefix sum >= 0: 若此時 min prefix sum < 0，我們就必須將一些-1 變成 +1，也就是 u 要增加到讓 min prefix sum = 0，而 u 增加 v 也要跟著增加，因此我們就可以確定 u + v 要是多少了
	
	    舉例來說，假設此時序列為 `- + - - + + + - + +`，算出 x = 6, y = 4，所以可以由 (x-y) + 2u - 2v = 0 得到 u - v = -1，我們將 u 設為 0，這時 u = 0, v = 1，目前的 prefix sum:
	
	    ```
	     -  +  -  -  +  +  +  -  +  -
	    -1  0 -1 -2 -1  0  1  0  1  0
	    ```
	
	    可以看到 min prefix sum 為 -2，則我們需要 u = 1 來使 min prefix sum >= 0，用 greedy 的策略從前面修改 u，從後面修改 v，得:
	
	    ```
	     +  +  -  -  +  +  +  -  -  -
	     1  2  1  0  1  2  3  2  1  0
	    ```
	
	    所以最後是 u = 1, v = 2
		
		---
		
	    另外一種方法是先使 min prefix sum >= 0，再調整讓 sum = 0。我們從第一項開始往後依序考慮，若當前右括號 - 左括號的數量 < 0 了，則將目前的右括號改成左括號，否則就往下一項看，這樣做到最後可以保證 min prefix sum >= 0，而且可能會多放了幾個左括號，也就是我們需要去增加 v 的數量，所以我們從後面往前將看到的右括號改成左括號直到 sum = 0 即可

???+note "合法判斷/最大匹配深度 [CF 1263 E. Editor](https://codeforces.com/contest/1263/problem/E)"
	現在有一個打字機，有以下操作 :
	
	- `L` : 將 pointer 往左移 1 格
	
	- `R` : 將 pointer 往右移 1 格
	
	- 一個小寫字母或者 `(`, `)` : 將 pointer 上的字元替換為給定字元
	
	在每次操作後，判斷這一行是否是合法括號序列。如果是的話，輸出最大匹配深度
	
	??? note "思路"
		對於一個區間而言，括號能否成功匹配有兩個判斷標準: 
		
		1. 左右括號數量要相同
		2. 任意前綴中，右括號的數目不能大於左括號的數目.
	
		如果我們把左括號看為 +1，右括號看為 -1，則上述標準等價於: 
		
		1. 區間和為 0
		2. 區間最小前綴和也應等於 0
	
		此時最大匹配深度應是區間最大連續子段和。假設區間可以正確匹配，則前綴和不會出現負數情況，因此最大連續子段和等價於最大前綴和，我們只需要維護最大前綴和即可。
		
		因此對於這類問題，我們只需要維護 :
		
		- 最大前綴和（最大深度）
	
		- 最小前綴和（判斷合法）
	
		- 區間和（判斷合法）
	
		> 參考 : <https://blog.csdn.net/weixin_45799835/article/details/120182104>

## 合法長度/方法數

???+note "[Leetcode 32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)"
	給一個長度為 n 的括號序列，問最長合法子字串（連續）長度

	$0\le n\le 3\times 10^4$

	??? note "code"
		```cpp linenums="1"
	    class Solution {
	    public:
	        int longestValidParentheses(string s) {
	            int res = 0, start = 0, n = s.size();
	            stack<int> st;
	            for (int i = 0; i < n; ++i) {
	                if (s[i] == '(') {
	                    st.push(i);
	                } else if (s[i] == ')') {
	                    if (st.empty()) {
	                        start = i + 1;
	                    } else {
	                        st.pop();
	                        res = st.empty() ? max(res, i - start + 1) : max(res, i - st.top());
	                    }
	                }
	            }
	            return res;
	        }
	    };
	    ```

???+note "合法子字串方法數 [51Nod 1791 合法括号子段](https://www.51nod.com/Challenge/Problem.html#problemId=1791)"
	給一個長度為 n 的括號序列，求合法子字串個數
	
	$n\le 1.1\times 10^6$
	
	??? note "思路"
		考慮 dp 求解，令 dp(i) 表示以 i 結尾的合法括號子字串個數，答案就會是 $\sum dp(i)$
	
	    使用 stack 對於每一個左括號開始記錄它的位置，當遇到一個右括號，它可以由和自己匹配的位置減去 1 的位置轉移而來，即
	    
	    $$
	    dp(i)=dp(j-1)+1
	    $$
	    
	    要 + 1 是因為可以選擇要不要繼續往前接，或是用跟自己匹配的就好。以這個例子來說 ( ) ( ( ) ( ) )，跟最後一個右括號匹配的左括號以藍色標註: ( ) <font color="#00A2E8">(</font> ( ) ( ) <font color="#00A2E8">)</font>，由於子字串一定要連續，所以只能從匹配的左括號之前繼續接
	    
	    > 參考自: <https://www.luogu.com.cn/blog/corleonefamily/kuo-hao-xu-lie-wen-ti>
	
	??? note "code"
	    ```cpp linenums="1"
	    #include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    int countValidSubstrings(string s) {
	        int n = s.length();
	        s = "$" + s;
	        vector<int> dp(n + 1, 0);
	        stack<int> stk;
	        int ans = 0;
	
	        for (int i = 1; i <= n; ++i) {
	            if (s[i] == '(') {
	                stk.push(i);
	            } else if (!stk.empty()) {
	                dp[i] = dp[stk.top() - 1] + 1;
	                stk.pop();
	                ans += dp[i];
	            }
	        }
	
	        return ans;
	    }
	
	    signed main() {
	        int t;
	        cin >> t;
	        while (t--) {
	            string s;
	            cin >> s;
	            cout << countValidSubstrings(s) << '\n';
	        }
	    }
	    ```

???+note "[AcWing 4207. 最长合法括号子序列](https://www.acwing.com/problem/content/4210/)"
	給一個長度為 $n$ 的括號序列，問最長合法子序列（不一定連續）長度
	
	$1\le n\le 10^6$
	
	??? note "思路"
		我們從第一項開始往後依序考慮，若當前右括號 - 左括號的數量 < 0 了，就不取當前這一項的右括號，否則就將 ans++
	
	??? note "code"
		```cpp linenums="1"
	    #include <cstdio>
	    #include <algorithm>
	    using namespace std;
	
	    const int N = 1e6 + 10;
	
	    char str[N];
	
	    int main() {
	        scanf("%s", str);
	
	        int cnt = 0, ans = 0;
	        for (int i = 0; str[i]; i++) {
	            if (str[i] == '(') {
	                cnt++;
	                ans++;
	            } else if (cnt > 0) {
	                cnt--;
	                ans++;
	            }
	        }
	
	        printf("%d", ans);
	
	        return 0;
	    }
	    ```

???+note "最長合法括號序列 [CF 380 C. Sereja and Brackets](https://www.luogu.com.cn/problem/CF380C)"
	給定一個長度 $n$ 的括號序列 $s$，有 $q$ 個詢問 :
	
	- $\text{query}(l, r):$ 輸出 $s_l,\ldots ,s_r$ 的最長合法括號序列長度
	
	$1\le n\le 10^6, 1\le m\le 10^5$
	
	??? note "思路"
		考慮一個括號序列，我們把能匹配的括號全都刪掉，剩下的括號一定形如 `))))))(((((((((`
	
		我們考慮用線段樹去維護。考慮記錄每個區間
		
		- 未匹配的左括號數量
	
		- 未匹配的右括號數量
	
		- 當前區間已經產生的貢獻和
	
		在合併兩個區間時: 新區間的貢獻 = 左區間原先的貢獻 + 右區間原先的貢獻 + 合併後新產生的貢獻。其中，合併後新產生的貢獻 = 2 * min(左區間未匹配的左括號數, 右區間未匹配的右括號數)。
		
		```cpp linenums="1"
		struct Node {
			Node *lc = nullptr;
			Node *rc = nullptr;
			int l, r;
			int sum, vl, vr;
			
			void pull() {
				int macthes = min(l->vl, r->vr);
				sum = lc->sum + rc->sum + 2 * macthes;
				vl = l->vl + r->vl - matches;
				vr = l->vr + r->vr - matches;
			}
		};
		```
		
		> 參考 : <https://blog.csdn.net/weixin_45799835/article/details/120037468>

???+note "合法括號子序列方法數"
	給一個長度為 n 的序列，求括號序列的合法「子字串」個數
	
	$n\le 5000$
	
	??? note "思路"
		dp(i, k) 表示考慮 1~i，左括號比右括號多 k 個的子序列數量
		
		轉移如下
		
		```
		dp(i, k) += dp(i - 1, k)
		if s[i] == '(': dp(i, k) += dp(i - 1, k - 1)
		else s[i] == ')': dp(i, k) += dp(i - 1, k + 1)
		```
		
## 其他

???+note "最少交換次數"
	給一個長度為 $n$ 的括號序列，每次可以 swap 相鄰的兩項，問至少幾次才能變合法

	$n\le 2\times 10^6$
	
	??? note "思路"
		利用 stack 處理括號的方式，令 cnt 為當前左括號 - 右括號的數量，若當前 cnt < 0，則要將當前最近的左括號給 swap 過來，具體來說，假設當前的 index 是 i，在右側離 i 最近的左括號在 j，則需要花 j - i 的 cost 將這個左括號給搬過來，然後我們再將 i, j swap 即可。實作上，可以用一個 queue 存所有左括號個位置，然後用一個指針在裡面單調往右移動即可，詳見代碼。
	
		> 參考自: <https://cloud.tencent.com/developer/news/395689>
	
	??? note "code"
		```cpp linenums="1"
	    #include <iostream>
	    #include <vector>
	    using namespace std;
	
	    long swapCount(string s) {
	        vector<int> pos;
	        for (int i = 0; i < s.length(); ++i) {
	            if (s[i] == '(') {
	                pos.push_back(i);
	            }
	        }
	
	        int counter = 0;
	        int p = 0; 
	        long sum = 0; 
	        for (int i = 0; i < s.length(); ++i) {
	            if (s[i] == '(') {
	                ++counter;
	                ++p;
	            } else if (s[i] == ')') {
	                --counter;
	            }
	            if (counter < 0) {
	                sum += pos[p] - i;
	                swap(s[i], s[pos[p]]);
	                ++p;
	                counter = 1;
	            }
	        }
	        return sum;
	    }
	
	    int main() { 
	        string s = "())()(";
	        cout << swapCount(s) << endl;
	
	        s = "(()())";
	        cout << swapCount(s) << endl;
	
	        return 0;
	    }
	    ```

???+note "[AcWing 3420. 括号序列](https://www.acwing.com/problem/content/3423/)"
	給一個長度為 n 的括號序列，盡可能少地添加若干括號使序列合法，問有幾種添加的方法數
	
	$n\le 5000$
	
	??? note "思路"
		**【分析，簡化問題】**
	
		左括號與右括號所新增的位置方案是相互獨立的，不會相互影響，即使左、右括號添加在同一個間隙，因為不能存在 "()" 的形式，此處只能為類似 "))((" 的一種形式，故總的方案數等於左括號的方案數 * 右括號的方案數。
		
		單獨考慮添加左括號，若以右括號為分割點，將整個序列進行分割，因為分割後的子字串中均為左括號，添加任意數目的左括號方案數均為一種，那麼此時，我們僅需考慮添加不同數量的左括號的方案數即可。右括號同理。
		
		**【前綴 dp】**
		dp(i, j) 表示只考慮前 i 部分，左括號比右括號多 j 個的所有方案數
		
		轉移如下:
		
		- 若 s[i] == '('，轉移式為 dp(i, j) = dp(i - 1, j - 1)
	
		- 若 s[i] == ')'，轉移式為 dp(i, j) = dp(i - 1, j + 1) + dp(i - 1, j) + ... + dp(i - 1, 0)。可以透過前綴優化得到 dp(i, j) = dp(i - 1, j + 1) + dp(i, j - 1)。為了防止越界，dp(i, 0) 需要特判。
		
		> 參考自: <https://www.acwing.com/solution/content/75383/>
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	
	    using namespace std;
	
	    typedef long long ll;
	
	    const int N = 5010, MOD = 1e9 + 7;
	    int n;
	    char str[N];
	    ll dp[N][N];
	
	    ll add(ll x, ll y) {
	        return (x + y) % MOD;
	    }
	
	    ll brackets() {
	        memset(dp, 0, sizeof dp);
	        dp[0][0] = 1;
	
	        for (int i = 1; i <= n; i++) {
	            if (str[i] == '(') {
	                for (int j = 1; j <= n; j++) {
	                    dp[i][j] = dp[i - 1][j - 1];
	                }
	            } else {
	                dp[i][0] = add(dp[i - 1][0], dp[i - 1][1]);
	                for (int j = 1; j <= n; j++) {
	                    dp[i][j] = add(dp[i - 1][j + 1], dp[i][j - 1]);
	                }
	            }
	        }
	
	        for (int i = 0; i <= n; i++) {
	            if (dp[n][i]) {
	                return dp[n][i];
	            }       
	        }
	        return -1;
	    }
	
	    int main() {
	        scanf("%s", str + 1);
	        n = strlen(str + 1);
	
	        ll ans_l = brackets();
	
	        reverse(str + 1, str + n + 1);
	        for (int i = 1; i <= n; i++) {
	            if (str[i] == '(') {
	                str[i] = ')';
	            } else {
	                str[i] = '(';
	            }
	        }
	        ll ans_r = brackets();
	
	        printf("%lld\n", ans_l * ans_r % MOD);
	
	        return 0;
	    }
	    ```

???+note "[2022 全國賽 pD. 文字編輯器 (editor)](https://nhspc2022.twpca.org/release/problems/problems.pdf#page=11)"
	有一個由 $\texttt{+}, \texttt{[}, \texttt{]}, \texttt{x}$ 組成合法序列，此時將其中一個 $\texttt{+}$ 改成 $\texttt{|}$，並將所有 $\texttt{[}, \texttt{]}$ 換成 $\texttt{|}$。給你這個改完的序列 $s$，輸出任意一個原來的合法序列。
	
	$|s| \le 10^6$
	
	??? note "思路"
		兩個 $\texttt{x}$ 中一定要有 $\texttt{+}$，看哪兩個 $\texttt{x}$ 之間沒有 $\texttt{+}$，Greedy 的放即可

???+note "<a href="/wiki/other/images/ioic_305.html" target="_blank">2023 IOIC 305 . 括號國</a>"
	給一個長度為 $n$ 的括號字串，問所有 substring 的「最長合法括號子序列長度」總和為何？
	
	$1\le n\le 2\times 10^5$
	
	??? note "思路"
		我們使用一般用 stack 括號序列的方式，若我們現在遇到了一個 closing，那前一個 opening 與目前這個 closing 的貢獻就是 opening 往前的長度 $\times$ closing 往後的長度，這些 l, r 在這些範圍內的都會算到我們
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    const int N = 2e5 + 5;
	    int st[N];
	
	    signed main() {
	        int n;
	        string s;
	        cin >> n >> s;
	        s = "$" + s;
	
	        int ans = 0, r = 0;
	        for (int i = 1; i <= n; i++) {
	            if (s[i] == '(') st[++r] = i;
	            else if (s[i] == ')' && r > 0) ans += 1ll * st[r--] * (n - i + 1);
	        }
	        cout << 2 * ans << '\n';
	    }
		```


???+note "[UVA 1626. Brackets Sequence](https://vjudge.net/problem/UVA-1626)"
	給一個長度 n 的括號序列 s，可任意增加括號，問最少增加幾次才能使 s 成為合法括號序列 ? 
	
	$n\le 100$
	
	??? note "思路"
		dp(l, r) = s[l..r] 最少要加入幾個括號可以變成合法的
		
		轉移的話我們分成看看能不能拆成前後兩段，不能的話代表一定是前後配對
		
		dp(l, r) = min{
		
		- dp(l, k) + dp(k + 1, r) // 可以分成前後兩段
	
		- dp(l + 1, r - 1) if ok(s[l], s[r]) // 前後兩個剛好可以配對
	
		- dp(l, r - 1) + 1 // 例如說 <font color="#00A2E8">[</font> ( [ ( ] 
	
		- dp(l + 1, r) + 1 // 例如說 ( [ ( ] <font color="#00A2E8">)</font>


???+note "[TOI 2021 二模 pC. 配對問題（Pairing）](https://drive.google.com/file/d/1aKGdK6ZjvXVAiXB5iH2eOiLdtC4w31j5/view)"
	給定一個序列 $a_1, a_2, ..., a_n$，一個點只能被匹配最多一次，當兩個點 $i$ 與 $j$ 配對時，就會獲得 $a_i + a_{i+1} + \cdots + a_j$ 的分數。任何配對都不能出現部份相交的情形。匹配結束後，所有沒有被匹配到的點 $i$ ，如果 $a_i > 0$，可以獲得 $a_i$ 分。問分數最大是多少

	$1 \leq n \leq 10^5, -10^9 \leq a_i \leq 10^9$
	
	??? note "思路"
		**【區間 dp: O(n^3)】**
	
	    dp(l, r) : l ~ r 的最多 cost
	
	    - dp(l+1, r) + a[l]
	
	    - dp(l, r-1) + a[r]
	
	    - 切兩半 dp(l, k) + dp(k+1, r)
	
	    - l 和 r 配對 dp(l+1, r-1) + (a[l]+...+a[r])
	
		---
	
	    **【前綴 dp: O(n^2)】**
	
	    dp(i, k) : 1~i 的 (#左括號 - #右括號) = k
	
	    ans = dp(n, 0)
	
	    轉移
	
	    - i 是 "(" : dp(i-1, k-1) - pre[i-1]
	
	    - i 是 ")" : dp(i-1, k+1) + pre[i]
	
	    - i 是 "X" : dp(i-1, k) + pre[i] - pre[i-1]
	
		---
		
		**【Greedy 觀察性質, 後悔法: O(n log n)】**
		
		如果有辦法匹配（與 pq.top 的貢獻是正的），就將目前這項選為右括弧，並 push 到 heap 中。若沒辦法匹配，也 push 到 heap 中。
		
		那麼要如何處理「不選」的貢獻呢，我們其實可以將有選的貢獻中加入 -a[i]，這樣就可以用扣得算了
		
		這邊提供一組範例
		
	    ```
	            i         1   2   3  4  5  6  7  8
	            a[i]:     3  -1  -5  3  8  4 -3  4
	            pre[i]:   3   2  -3  0  8 12  9 13
	    ```
		
		> 參考自 : [TOI 2021 Solutions - p3 pairing](https://omeletwithoutegg.github.io/2021/09/22/toi-2021-sols/)

----

## 參考資料

- 進階延伸
	- <https://taodaling.github.io/blog/2020/12/07/%E6%8B%AC%E5%8F%B7%E9%97%AE%E9%A2%98/>
	- <https://www.cnblogs.com/Zeardoe/p/16213505.html>