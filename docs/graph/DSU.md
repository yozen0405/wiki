## 介紹

- 持久化並查集
- 帶權並查集 (CF imposter)
- 可撤銷並查集 (2023 成大賽 p4)
- 加點技巧 (海牛 class 11 hm)

## 連結

- 二分圖
- 啟發式合併
	- 海牛 class 11
	- DSU 啟發式合併複雜度證明 (知乎)
	- 啟發式合併複雜度 (oi wiki)
- 持久化線段樹

## 用途

???+note "[洛谷 P2024 [NOI2001] 食物链](https://www.luogu.com.cn/problem/P2024)"
	有三類動物 $A,B,C$，這三類動物的⾷物鏈構成如下：$A$ 吃 $B$，
	$B$ 吃 $C$，$C$ 吃 $A$。
	
	現有 $N$ 個動物，編號 $1,2,...,N$。每個動物都是 $A,B,C$ 中的⼀種，但並不知道是哪⼀種。依序給 $K$ 條屬
	於以下兩種的敘述：
	
	- $1\space X\space Y$，表⽰ $X$ 和 $Y$ 是同類
	
	- $2\space X\space Y$，表⽰ $X$ 吃 $Y$
	
	然⽽，並不是每條描述都是正確的，有些是真話，有些是假話。
	如果當前的話與前⾯的某些真話衝突，就是假話請判斷哪些話是真話，哪些話是假話。
	
	??? note "思路"
		- 我們可以⽤並查集維護資訊間的因果關係
	
	    - ⼀個集合裡⾯的資訊代表「這些資訊必須同時發⽣」
	
	    - ⽐如說，假設 $(x,y)$ 符合第⼀種資訊
	
	    - 那就代表：如果 $x$ 是 $A$ 類，那 $y$ 必須是 $A$ 類，反之亦然
	    （$B,C$ 類同樣可以套⽤）
	    
	    - 翻譯⼀下：如果 $x_A$ 發⽣的話，那 $y_A$ 也⼀定要發⽣（$B,C$
	    類同樣可以套⽤）
	    
	    - $\text{merge}(x_A, y_A),\text{merge}(x_B, y_B), \text{merge}(x_C,y_C)$
	
		---
		
		- ⽐如說，假設 $(x, y)$ 符合第⼆種資訊
	
	    - 那就代表：
	    	- 如果 $x$ 是 $A$ 類，那 $y$ 必須是 $B$ 類，反之亦然
	    	- 如果 $x$ 是 $B$ 類，那 $y$ 必須是 $C$ 類，反之亦然
	    - 如果 $x$ 是 $C$ 類，那 $y$ 必須是 $A$ 類，反之亦然
	
	    - $\text{merge}(x_A, y_B),\text{merge}(x_B, y_C), \text{merge}(x_C,y_A)$ 
		
		---
		
		- 矛盾的情況呢 ?
		- 假如 $(x, y)$ 符合第⼆種資訊
		- 如果 $\text{find}(x_A)=\text{find}(y_C)$ 或 $\text{find}(x_A)=\text{find}(y_A)$ 則矛盾
		
		---

		
		他們每個 col 將會以 $A \rightarrow B \rightarrow C$ 的順序旋轉
		所以當你知道其中一個關係的時候其實就能推得其餘的關係
		
		例如今天 $1$ 吃 $2$，$2$ 吃 $3$ 關西如下圖
		
		<figure markdown>
	      ![Image title](./images/3.png){ width="300" }
	      <figcaption>同一 row 屬於同一連通塊，必同時發生</figcaption>
	    </figure>
	    
	    <figure markdown>
	      ![Image title](./images/4.png){ width="200" }
	      <figcaption>圖(一)</figcaption>
	    </figure>
		
		因為 $1$ 吃 $2$，那麼以圖(一)來看就是 $1$ 是 $A$ 時，$2$ 就是 $B$
		
		可以看做 $1,2$ 分別以 $A,B$ 為起點，繞著關係圖轉一圈
		
		- 當 $1$ 在 $A$ 時，$2$ 在 $B$
		
		- 當 $1$ 在 $B$ 時，$2$ 在 $C$
	
		- 當 $1$ 在 $C$ 時，$2$ 在 $A$
	
		可以看到 $2$ 也是繞著關係圖走一圈的，所以當你確定他其中一步時，就能確定其他步
	
		---
		
		```cpp linenums="1"
		if (flag == 1) {
			if (check (a, b + n) || check (a, b + 2*n)) {
				ans++;
			}
			else {
				merge (a, b), merge (a + n, b + n), merge (a + 2*n, b + 2*n);
			}
		}
		else {
			if (check (a, b) || check (a, b + 2*n)) {
				ans++;
			}
			else {
				merge (a, b + n), merge (a + n, b + 2*n), merge (a + 2*n, b);
			}
		}
		```


### Uva 11987

???+note "[zerojudge f292. 11987 - Almost Union-Find](https://zerojudge.tw/ShowProblem?problemid=f292)"
	有個 $n$ 物品，每個物品一開始都是自己一組
	
	有 $q$ 次操作，每次會是其中一種
	
	- $\text{Merge}(x,y):$ 將 $x,y$ 所在的兩個群體合併為同一個
	
	- $\text{MoveGroup}(x,y):$ 將 $x$ 從他所在的群體當中移除並且加入 $y$ 所在的群體
	
	- $\text{Sum}(x):$ 印出 $x$ 所在的群體包含的成員個數和成員編號總合
	
	??? note "思路"
		我們先來思考「將 $x$ 從他所在的群體當中移除」
		
		我們直接將 $x$ 的貢獻給扣掉，也不必真正在 DSU 裡將其刪除
		
		接下來思考 「並且加入 $y$ 所在的群體」
		
		我們可以對於 $i=1\sim n$ 維護 $t_i$ 代表目前 $i$ 真正的編號
		
		加入新的 group 的時候只需將 $t_i$ 變成當前沒用過的編號即可，並且可以當成是一個新的點，去執行 merge
		
		詳見代碼
		
		??? note "code"
			```cpp linenums="1"
	        void init () {
	            for (int i = 1; i <= n; i++) {
	                f[i] = i;
	                t[i] = i;
	                sum[i] = i;
	                num[i] = 1;
	            }
	            cnt = n;
	        }
	
	        void delete (int x) {
	            sum[find (t[x])] -= x;
	            num[find (t[x])] -= 1;
	
	            t[x] = ++cnt;
	            sum[t[x]] = x;
	            num[t[x]] = 1;
	            f[t[x]] = t[x];
	        }
	
	        void merge (int x, int y) {
	            int tx = find (t[x]);
	            int ty = find (t[y]);
	            if (tx != ty) f[ty] = tx;
	            num[tx] += num[ty];
	            sum[tx] += sum[ty];
	        }
	
	        void solve (int x, int y) {
	            delete (x);
	            merge (x, y);
	        }
	        ```
			> 參考自 : [CSDN](https://blog.csdn.net/weixin_52914088/article/details/120379127?ops_request_misc=&request_id=&biz_id=102&spm=1018.2226.3001.4187)

???+note "[CF 1594 D. The Number of Imposters](https://codeforces.com/problemset/problem/1594/D)"
	給定 $n$ 個點，每個點有一個未知的 $w_i\in \{0,1\}$，再給 $m$ 個關係
	
	- $(x,y,0):$ $w_x = w_y$
	- $(x,y,1):$ $w_x \neq w_y$
	
	判斷最多有多少個點的 $w_i=1$


	??? note "思路 1"
		考慮二分圖染色法判斷，兩個點之間有邊代表兩點的顏色不同
		
		- $(x,y,0):$ 在 $x,y$ 之間建一條邊
		- $(x,y,1):$ 建立一個 $z$ 點分別連接 $x,y$
		
		這樣下去跑二分圖染色法即可，每個連通塊答案取兩種顏色的 $\max$，再加起來
		注意在跑二分圖染色法時不能將多餘的 $z$ 點算進去
	
		??? note "code(from [acwing](https://www.acwing.com/file_system/file/content/whole/index/content/3069744/))"
	        ```cpp linenums="1"
	        #include <bits/stdc++.h>
	        using namespace std;
	        typedef long long ll;
	        typedef pair<ll, ll> pii;
	        const int N = 7e5 + 10;
	        const int M = 2e6 + 8e5 + 10;
	        int c[2];
	        int n, z;
	        int e[M], ne[M], h[N], idx;
	        int st[N];
	
	        void add(int a, int b) {
	            e[idx] = b, ne[idx] = h[a], h[a] = idx++;
	        }
	
	        void init() {
	            idx = 0;
	            for (int i = 1; i <= z; i++)
	            {
	                h[i] = -1;
	                st[i] = 0;
	            }
	        }
	        bool dfs(int u, int color) {
	            st[u] = color;
	            if (u <= n) // 不算入 z 點
	                c[2 - color]++;
	
	            for (int i = h[u]; i != -1; i = ne[i]) {
	                int j = e[i];
	                if (!st[j]) {
	                    if (!dfs(j, 3 - color))
	                        return false;
	                }
	                else if (st[j] == color)
	                    return false;
	            }
	            return true;
	        }
	
	        void slove() {
	            int m;
	            cin >> n >> m;
	
	            z = n + 1;
	            for (int i = 1; i <= m; i++) {
	                int a, b;
	                char c[10];
	                scanf("%d %d %s", &a, &b, c);
	
	                if (c[0] == 'c') {
	                    add(a, z);
	                    add(z, a);
	                    add(b, z);
	                    add(z, b);
	                    z++;
	                }
	                else {
	                    add(a, b);
	                    add(b, a);
	                }
	            }
	            int ans = 0;
	            int ok = true;
	            for (int i = 1; i <= n; i++) {
	                if (!st[i]) {
	                    c[0] = 0;
	                    c[1] = 0;
	                    bool flag = dfs(i, 1);
	
	                    if (!flag) {
	                        ok = false;
	                        break;
	                    }
	
	                    ans += max(c[0], c[1]);
	                }
	            }
	            if (!ok)
	                ans = -1;
	
	            cout << ans << endl;
	            init ();
	        }
	
	        int main() {
	            int Q;
	            cin >> Q;
	            memset(h, -1, sizeof h);
	            while (Q--) {
	                slove();
	            }
	            return 0;
	        }
	        ```
	        
	??? note "思路 2"
		考慮帶權並查集
		
		對於每個並查集維護並查集內每個點與 root 的距離是 $0$ 或是 $1$
		
		最後每個並查集 $\max($與 root 的距離是 $0$ 的數量 $,$ 與 root 的距離是 $1$ 的數量$)$ 
		
		??? note "code"
	        ```cpp linenums="1"
	        #include <bits/stdc++.h>
	        #define int long long
	        #define pb push_back
	        #define mk make_pair
	        #define F first
	        #define S second
	        #define pii pair<int, int>
	        using namespace std;
	
	        const int INF = 9e18;
	        const int maxn = 2e5 + 5;
	        int n, m;
	        int dis[maxn], par[maxn], cnt[maxn][2];
	
	        int find (int x) {
	            if (par[x] == x) return x;
	            else {
	                int root = find (par[x]);
	                dis[x] ^= dis[par[x]];
	                par[x] = root;
	                return root;
	            }
	        }
	
	        void solve () {
	            cin >> n >> m;
	            for (int i = 1; i <= n; i++) dis[i] = 0, par[i] = i, cnt[i][0] = 1, cnt[i][1] = 0;
	
	            string s;
	            int fg = 0;
	
	            for (int i = 1, u, v; i <= m; i++) {
	                cin >> u >> v >> s;
	
	                int dif;
	                if (s[0] == 'i') dif = 1; 
	                else dif = 0; // same
	
	                int x = find (u), y = find (v);
	                if (x == y) {
	                    if ((dis[u] ^ dis[v]) != dif) fg = 1;
	                }
	                else {
	                    dis[y] = dis[u] ^ dis[v] ^ dif;
	                    par[y] = x;
	                    cnt[x][0] += cnt[y][dis[y]];
	                    cnt[x][1] += cnt[y][dis[y] ^ 1];
	                }
	            }
	
	            if (fg == 1) {
	                cout << -1 << "\n";
	                return;
	            }
	
	            int res = 0;
	            for (int i = 1; i <= n; i++) {
	                if (find(i) == i) {
	                    res += max (cnt[i][0], cnt[i][1]);
	                }
	            }
	            cout << res << "\n";
	        }
	
	        signed main () {
	            int t;
	            cin >> t;
	            while (t--) {
	                solve ();
	            }
	        }
	        ```
	        
### 組別最大編號

???+note "海牛 class11 P9"
	有個 $n$ 物品編號依序是 $1,2,3,...,n$，每個物品一開始都是自己一組
	
	有 $q$ 次操作，每次會是其中一種
	
	- $\text{Merge}(x,y):$ 將 $x,y$ 所在的兩個群體合併為同一個
	
	- $\text{MoveGroup}(x,y):$ 把包含物品 $x$ 與物品 $y$ 的兩個組別合併成一個
	
	- $\text{GroupMax}(x):$ 求跟物品 $x$ 同一組的物品中，編號最大的物品編號

	$n,q\le 2\times 10^5$
	
	??? note "思路"
		維護很多個 priority_queue
		
        每個 pq 裡面存很多 $\texttt{pair}(x, t)$，$x$ 就是有的元素，$t$ 是時間戳記

        每次 Move 不要真的把東西搬到別的 Group, 而是直接新增一個時間戳記比較大的 $(x, t')$

        找最大值的時候，一直看這個 pq 的 $\max$
        
        如果時間戳記已經過期了就丟掉元素，一直到找到一個不是過期的元素




- DSU 判環
- https://blog.csdn.net/boliu147258/article/details/92778897
- <https://www.luogu.com.cn/problem/P3430>
- [TIOJ 只走一次](https://tioj.ck.tp.edu.tw/problems/2161)
	- [submission](https://tioj.ck.tp.edu.tw/submissions/311160)

---

## 參考資料

- <https://zhuanlan.zhihu.com/p/553192435>
- [Codeforces Edu DSU (需加入 group)](https://codeforces.com/edu/course/2/lesson/7)