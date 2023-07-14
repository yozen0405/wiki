又稱頂環樹，水母圖。中國稱基環樹。英文叫做 functional graph。

## 性質

內向基環樹符合以下性質

1. 每個點的 out degree 都是 1
2. 恰有 $n$ 條邊

<figure markdown>
  ![Image title](https://user-images.githubusercontent.com/71330526/204506183-b92d0490-06ce-4b5b-a2cc-33246ed2ffb2.png){ width="300" }
</figure>


- 一張圖每個點的 out degree 都是 1，必為內向基環樹
- 一張圖恰有 $n$ 條邊，必為基環樹

## 例題

???+note "[Atcoder abc256 E. Takahashi's Anguish](https://atcoder.jp/contests/abc256/tasks/abc256_e)"
	你要給 $n$ 個人分糖果，第 $i$ 個人都有一個嫉妒的人 $X_i$，若 $X_i$ 比 $i$ 先拿到糖果那會產生嫉妒值 $C_i$。在所有可能的順序裡面，輸出嫉妒值之和最小可以是多少
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		類似 [CSES - Course Schedule](https://cses.fi/problemset/task/1679)，我們建邊 $i\to X_i$，可以發現每個點的 out degree 都是 1，會形成內向基環樹。用 CSES 那題的想法一樣，用 topo sort 拔點，最後會剩環，代表我們需要捨棄掉環上的一條邊，讓 topo sort 得以進行，那我們當然是拔邊權最小的邊即可。所以答案就是每個內向基環樹上的環上的最小值總和

???+note "[洛谷 P4381 [IOI2008] Island](https://www.luogu.com.cn/problem/P4381)"
	一共有 $n$ 個島，每個島都有一條出邊，且該圖是無向圖，因為橋是可以雙向行走的。給定橋的長度，即兩點之間的邊權。同時每對島嶼間存在一艘專用渡船，即每兩點間可以相互到達。現你需選擇一起始點，每個點最多經過 $1$ 次，問所能獲得的邊權和最大為多少。
	
	$n\le 10^6$

???+note "[2022 TOI  模擬賽第一場 pA. Functional Graph](/wiki/cp/contest/images/TOI-2022-1.pdf#page=1)"
	給一張 $n$ 點 $m$ 邊的基環樹，將邊定向，並輸出最小字典序的方向序列
	
	$n\le 10^5$

???+note "[CSES - Round Trip II](https://cses.fi/problemset/task/1678)"
	給一張 $n$ 點 $m$ 邊無向圖，輸出環上的點
	
	$n\le 10^5,m\le 2\times 10^5$
	
	??? note "思路"
		我們想法是先把這張圖變成一張基環樹。找 cycle 只需要用 topo sort 把  in degree = 0 的點刪掉讓圖上只剩環即可
		
		所有 outdegree 是 0 的點一直移除，做完之後，所有人的 outdegree 都至少 1。對於每個剩下的點，把指出去的邊 （out-going edge） 只保留一條，就會變成基環樹了
		
	??? note "code"
		```cpp linenums="1"
	    #include <bits/stdc++.h>
	    #define int long long
	    #define pb push_back
	    #define mk make_pair
	    #define F first
	    #define S second
	    using namespace std;
	
	    const int INF = 2e18;
	    const int maxn = 1e5 + 5;
	    int n, m;
	    vector<int> G[maxn], R[maxn], H[maxn], F[maxn];
	    int out[maxn], in[maxn], vis[maxn];
	    vector<int> ans;
	
	    void init () {
	        cin >> n >> m;
	        int u, v;
	        for (int i = 0; i < m; i++) {
	            cin >> u >> v;
	            G[u].pb(v);
	            R[v].pb(u);
	            out[u]++;
	        }
	    }
	
	    void del () {
	        queue<int> q;
	        for (int i = 1; i <= n; i++) {
	            if (out[i] == 0) q.push(i);
	        }
	
	        while (q.size()) {
	            int u = q.front();
	            q.pop();
	            for (auto v : R[u]) {
	                out[v]--;
	                if (out[v] == 0) {
	                    q.push(v);
	                }
	            }
	        }
	    }
	
	    void topo () {
	        queue<int> q;
	        for (int i = 1; i <= n; i++) {
	            if (in[i] == 0 && out[i] >= 1) q.push(i);
	        }
	
	        while (q.size()) {
	            int u = q.front();
	            q.pop();
	            for (auto v : H[u]) {
	                in[v]--;
	                if (in[v] == 0) {
	                    q.push(v);
	                }
	            }
	        }
	    }
	
	    void dfs (int u) { // print cycle
	        ans.pb(u);
	        if (vis[u]) {
	            cout << ans.size() << "\n";
	            for (auto ele : ans) cout << ele << " ";
	            exit(0);
	        }
	        vis[u] = true;
	        for (auto v : F[u]) {
	            dfs (v);
	        }
	    }
	
	    void solve () {
	        del(); // delete out[i] == 0
	        for (int i = 1; i <= n; i++) {
	            if (out[i] >= 1) {
	                for (auto v : G[i]) {
	                    if (out[v] >= 1) {
	                        H[i].pb(v); // 頂環樹
	                        in[v]++;
	                        break;
	                    }
	                }
	            }
	        }
	        topo (); // only keep the cycle
	        int tmp = -1;
	        for (int i = 1; i <= n; i++) {
	            if (out[i] >= 1 && in[i] >= 1) {
	                tmp = i;
	                for (auto v : H[i]) {
	                    if (out[v] >= 1 && in[v] >= 1) {
	                        F[i].pb(v); // only cycle graph
	                        break;
	                    }
	                }
	            }
	        }
	        if (~tmp) dfs (tmp);
	        cout << "IMPOSSIBLE\n";
	    }
	
	    signed main () {
	        ios::sync_with_stdio(0);
	        cin.tie(0);
	        init ();
	        solve ();
	    }
	    ```

???+note "[CSES - Planets Queries I](https://cses.fi/problemset/task/1750)"
	給一張 $n$ 點內向基環樹，$q$ 筆詢問 :
	
	- 從 $x$ 點走 $k$ 步，到達哪個點
	
	$n,q\le 2\times 10^5,k\le 10^9$

???+note "[CSES - Planets Queries II](https://cses.fi/problemset/task/1160)"
	給一張 $n$ 點內向基環樹，$q$ 筆詢問 :
	
	- 從 $a$ 走到 $b$ 的最少步數，或走不到
	
	$n,q\le 2\times 10^5$

???+note "[CSES - Planets Cycles](https://cses.fi/problemset/task/1751)"
	給一張 $n$ 點內向基環樹，輸出每個點出發需要走幾步才能碰到已經 visited 過的點

	$n,q\le 2\times 10^5$

???+note "水母圖直徑 [Zerojudge k186. pC. 關卡地圖 (game)](https://zerojudge.tw/ShowProblem?problemid=k186) "
	給一張 $n$ 點 $m$ 邊無向圖，問最長權重和的 path 權重和是多少
	
	$n\le 10^5,m=n-1$ 或 $n$

???+note "[洛谷 P1543 [POI2004] SZP](https://www.luogu.com.cn/problem/P1543)"
	給一個 $n$ 點，問至多可以選幾個點，滿足每個被選的點至少是一個沒選的點的 out degree
	
	$n\le 10^6$
	
	??? note "思路"
		[題解](https://blog.csdn.net/weixin_30689307/article/details/101170191?spm=1001.2101.3001.6650.5&utm_medium=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-101170191-blog-93249322.wap_relevant_t0_download&depth_1-utm_source=distribute.wap_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-5-101170191-blog-93249322.wap_relevant_t0_download)

???+note "[CF 1270 G. Subset with Zero Sum](https://codeforces.com/contest/1270/problem/G)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，選一些數使得總合為 $0$，印出這些數的 index，保證有解
	
	$n\le 10^6,i-n\le a_i\le n-1$
	
	??? note "思路"
		通靈解。
		
		轉變一下式子得到：$1\le i-a_i \le n$，對每個 $a_i$ ，建一條從 $i$ 到 $i-a_i$ 的邊，由於每個點都有出度，整個圖必定有環。對於環可以得到：
	    
	    $$\begin{align}& i_1 - a_1=i_2 \\ & i_2 - a_2=i_3 \\ & i_3 - a_3 = i_4 \\ &\cdots \\ & i_n - a_n = i_1\end{align}$$
	    
	    求和得到 $a_1+a_2+\ldots +a_n=0$，因此環上所有點即是解，在這個限制條件下必定有解
	
	??? note "code (by rahlin)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int,int>
	    #define pb push_back
	    #define f first
	    #define s second
	    using namespace std;
	
	    const int INF=1e18, MAXN=1e6+100;
	    int t,n;
	    vector<int> a(MAXN),in(MAXN),vis(MAXN);
	
	    signed main(){
	        ios_base::sync_with_stdio(0);
	        cin.tie(0);
	        cin>>t;
	        while(t--){
	            cin>>n;
	            for(int i=1;i<=n;i++){
	                in[i]=0,vis[i]=0;
	            }
	            for(int i=1;i<=n;i++){
	                cin>>a[i];
	                a[i]=i-a[i];
	                in[a[i]]++;
	            }
	            queue<int> q;
	            for(int i=1;i<=n;i++) {
	                if(in[i]==0) q.push(i);
	            }
	            while(q.size()){
	                int u=q.front();
	                q.pop();
	                vis[u]=1;
	                int v=a[u];
	                if(!vis[v]){
	                    in[v]--;
	                    if(in[v]==0) q.push(v);
	                }  
	            }
	            int sum=0;
	            for(int i=1;i<=n;i++){
	                if(!vis[i]) sum++;
	            }
	            cout<<sum<<"\n";
	            for(int i=1;i<=n;i++){
	                if(!vis[i]) cout<<i<<" ";
	            }
	            cout<<"\n";
	        }
	    }
	    ```

---

## 資料

- <https://en.m.wikipedia.org/wiki/Pseudoforest>

- <https://usaco.guide/silver/func-graphs?lang=cpp#example---badge>

- <https://zhuanlan.zhihu.com/p/559456187>

- <https://www.cnblogs.com/cly-none/p/9314812.html>

- <https://www.cnblogs.com/A-sc/p/13554630.html>

- <https://www.luogu.com.cn/blog/ShadderLeave/ji-huan-shu-bi-ji>