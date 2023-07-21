## job sheduling

### k job scheduling

???+note "[CSES - Movie Festival II](https://cses.fi/problemset/task/1632)"
	給你 $n$ 個工作 $l_i,r_i$ 每個工作的 $w_i$ 都是 $1$，一次可以作 $k$ 個工作，求最多可以領多少錢
	
    ??? note "思路"
        - 如果同時間有好幾個工作重疊並且 $>k$
    
        - 貪心想法，我們把 $r$ 最右邊的刪掉
    
        - 從左掃到右因為我們在這之前都處理好了(重疊不超過 $k$ ) 所以你左邊不管怎麼延伸我都沒差, 現在右邊的還沒處理所以我們要看的是 $r$
    
        - 如果有多個最大的 $r$ 一樣那你刪哪一個其實都沒差(左邊都處理好了)
    
    ??? note "code"
        ```cpp linenums="1"
        int n;
        multiset<pii> ms;
    
        void init () {
            cin >> n;
            for (int i = 1, l, r; i <= n; i++) {
                cin >> l >> r;
                ms.insert({l, r});
            }
        }
    
        int solve (int k) {
            // 一次做 k 個工作歐布歐虧
            multiset<pii> rev;
            int W = n;
            for (auto pos = ms.begin(); pos != ms.end(); pos++) {
                int l = pos->first, r = pos->second;
                //以我目前的 L 來看重疊的左屆
                // 刪除過期的邊界
                while (rev.size() && rev.begin()->first < l) {
                    rev.erase(rev.begin());
                }
                rev.insert({r, l});
                // 保證大於 k 的大小只會剛好是 k + 1
                // 因為一到 k + 1 就會被刪掉一個
                if (rev.size() > k) {
                    rev.erase(--rev.end());
                    // rev.end() 就是 r 最大的, 必須刪掉
                    W--;
                }
            }
            return W;
        }
        ```
        
???+note "[TIOJ 1337. 隕石]"
	給 $n$ 個 interval $l_i,r_i$，問在可以移除 $k$ 個 interval 的情況最小化最大的 bandwidth
	
	$n\le 10^5,K$
	
	??? note "思路"
		二分搜答案 t，也就是最大的 bandwidth，去 check(t) 說可不可以再刪掉 k 個 interval 的情況下使最大 bandwidth 不超過 t
	
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        #define int long long
        #define pb push_back

        using namespace std;
        using pii = pair<int, int>;

        int n, m;
        multiset<pii> ms;

        void init () {
            cin >> n >> m;
            for (int i = 1, l, r; i <= n; i++) {
                cin >> l >> r;
                ms.insert({l, r});
            }
        }

        int check (int k) {
            // 一次做 k 個工作歐布歐虧
            // 但刪掉的工作不能超過 m 個
            // 東區那題要刪多少都ok
            multiset<pii> rev;
            int cnt = m;
            for (auto pos = ms.begin(); pos != ms.end(); pos++) {
                int l = pos->first, r = pos->second;
                //以我的角度來看重疊在左屆
                // 刪除過期的邊界
                while (rev.size() && rev.begin()->first <= l) {
                    rev.erase(rev.begin());
                }
                rev.insert({r, l});
                if (rev.size() > k) {
                    if (cnt == 0) return false;
                    rev.erase(--rev.end());
                    cnt--;
                }
            }
            return true;
        }

        void solve () {
            int l = 0, r = n - m;
            while (l < r) {
                int mid = l + r >> 1;
                if (check(mid)) r = mid;
                else l = mid + 1;
            }
            cout << r;
        }

        signed main () {
            ios::sync_with_stdio(0);
            cin.tie(0);
            init();
            solve();
        }
        ```

### job scheduling problem
???+ note "延伸 job scheduling problem"
	給 n 個 intervals，有 weight，找一些 intervals，兩兩不 overlap，最大化 weight 總和

	??? note "思路1"
		- sort $r_i$ 小到大
		- $dp(i)$ 為從 $0$ ~ $i$ 有挑 $i$ 的答案
		
		$$dp(i)=w(i)+\max \limits_{r_j \text{ }< \text{ }l_i} \{dp(j)\}$$
		
		- $r_j < l_i$ 一定是一段 prefix
	
		- 複雜度 $O(n\log n)$
		
	??? note "思路2"
		- $dp[i]=$ 從第 $0$ 項 ~ 第 $i$ 項不一定要挑第 $i$ 項的答案
	
		- $dp[i]=\max\{dp[i-1],dp[j]+w[i]\}$
	
		- 二分搜查找 $j$

## machine scheduling

### 最多能完成幾個工作
???+note "[洛谷 P4053 [JSOI2007] 建筑抢修](https://www.luogu.com.cn/problem/P4053)"
	- $n$ 個工作，每個工作有需要執行的時間 $t_i$ 與截止時間 $d_i$

	- 工作若在截止時間前完成就可以獲得報酬 $1$，否則報酬是 $0$
	
	- 你可以自由安排工作的順序，目標是最大化報酬總和
	
	- $1 \le n \le 2\times10^5, 1 \le a \le b \le10^9$
	  
	??? note "思路"
		- 先想如何判斷 $n$ 個工作是否都可以做完?
	
		- 將工作先按照截止時間排序
	
		> Proof : 
		
		> 若有兩個工作 $A, B$，$A$ 的 deadline 在 $B$ 之前，那麼 $t[A] + t[B]$ 一定都一樣，不管是 $A\rightarrow B$ 和 $B\rightarrow A$，對於 $B$ 的 deadline 的影響都是一樣的，所以最重要的是 $A$ 的 deadline，那我們先將 $A$ 先做越有機會在 $A$ 的 deadline 前完成，故 deadline 小的放越前面
	
		- 若前 $i$ 個工作可以選 $k$ 個在截止時間前完成
	
		- case 1 : 前 $i+1$ 個工作可以選 $k+1$ 個在截止時間內完成，最小完成時間 ($\sum t[i]$) 是 $S$
	  		- 直接將第 $i+1$ 個工作選起來
	  		- $S + t[i + 1] \le d[i+1]$
	
		- case 2 : 前 $i+1$ 個工作只能選 $k$ 個在截止時間內完成
	  	- $S'=\min (S,$ 把原本的 $k$ 個刪掉其中一個, 然後加入 $t[i+1])$
	  	- 維護目前已選擇的工作，支援刪除 $t[i]$ 最大的 $\Rightarrow$ 反悔法
	
	??? note "code"
		```cpp linenums="1"
		const int N = 10;
	    struct Job {int time, due;} job[N];
	    priority_queue<int> pq;
	
	    bool cmp(Job& a, Job& b)
	    {
	        return a.due < b.due;
	    }
	
	    void solve () {
	        sort(job, job + N, cmp);
	
	        int t = 0;
	        for (int i = 0; i < N; i++) {
	            pq.push(job[i].time);
	            t += job[i].time;
	            if (t > job[i].due) t -= pq.top(), pq.pop();
	        }
	
	        cout << pq.size() << "\n";
	    }
	    ```
### 最小化截止時間
???+ note "最小化截止時間之和"
	給定 $n$ 個工作，第 $i$ 個工作有需要做的時間 $t[i]$，最小化每個工作完成的時間之和
	??? note "思路"
		其實只要 $\texttt{sort}(t[i])$ 就好了，越早做的會被累計最多次

???+ note "最小化截止時間之權重和"
	- 給定 $n$ 個工作，第 $i$ 個工作有需要做的時間 $t[i]$ 和權重 $w[i]$
	
	- 令 $C[i]$ 為第 $i$ 個工作完成的時間，最小化 $\sum C[i]\times w[i]$
	??? note "思路"
		$\texttt{sort}(t[i]/w[i])$ 就好了
		
		> proof :
		
	    > assume that the order $i\rightarrow j$ is optimal than $j\rightarrow i$<br>
	    > $t[i] \times w[i] + (t[i] + t[j]) \times w[j] < t[j] \times w[j] + (t[j] + t[i]) \times w[i]$<br>
	    > $\Rightarrow t[i] \times w[j] < t[j] \times w[i]$<br>
	    > $\Rightarrow t[i] / w[i] < t[j] / w[j]$<br>

