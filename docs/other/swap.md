在 $n=20$ 的這種子題時，我們可以使用 bitmask dp。例如 dp(mask, i) 定義為已經放完了 mask 這個集合內的這些數字，而我最後放的是 $a_i$（才能得知順序）。例如當前的序列是 $a_1, a_2, a_3, a_4,a_5$，而當前轉移是 dp(\{1, 2, 4, 5\}, 2) = dp(\{1, 4, 5\}, 1) + 2，因為 $a_4, a_5$ 一開始在  $a_2$ 的後面，移到前面會產生貢獻。

## TOI 2019 p4

???+note "[TOI 2019 p4. 雲霄飛車 (rollercoaster)](https://sorahisa-rank.github.io/oi-toi/2019/problems.pdf#page=5) / [CSES - Pyramid Array](https://cses.fi/problemset/task/1747)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，問至少 swap 相鄰的幾次可使陣列先遞增，再遞減
	
	$n\le 10^5$
	
	??? note "思路"
		我們考慮 1，1 一定只能放在最左或最右，看哪邊比較近就放那邊，然後就可以移除 1，變子問題
		
		實作上我們需要一個 data structure 可以算:
		
		- 左邊有幾個 > i
	
		- 右邊有幾個 > i
	
		可以使用 BIT，複雜度 O(n log n)

## CF Split Sort

???+note "[CF 1863 B. Split Sort](https://codeforces.com/contest/1863/problem/B)"
	給一個 $1\sim n$ 的 permutation，輸出最少操作使 permutation 變 $(1,2,\ldots ,n)$
	
	- 選一個 $x$，把 $\le x$ 的在順序不變下拉到前面，再把 $>x$ 的在順序不變下拉到後面
	
	$n\le 10^5$
	
	??? note "思路"
		一個直覺的想法就是從 1 -> n 看上來，若出現逆序的話答案就要 +1。例如 1, 2, 3 都 ok，到 4 的時候發現往左走了，那我們勢必要花一次操作將 4 弄到 3 的右邊去。
		
		---
		
		> 之前想的思路 :
	
		考慮哪個時候要切 x。在 sorted 的 permutation 中，
	
		先考慮 1，若 1 不是在最前面，那我們有兩種選擇 :
	
	    - 花一次操作使 1 移動到第一格
	    - 讓以後的數字帶我到第一格
	        - 考慮從 x 切，那勢必 pos[1] 要小於 pos[2], ..., pos[x] 才能被帶飛
	        - 顯然當 x 越大的時候越難符合，所以我們其實只要考慮 x 最小，也就是 x = 2 的時候
	        - iff pos[1] < pos[2]
	
	    當我們選擇完後，即可將 1 移除，變成子問題
	    
	    總結下來，我們其實只要看 pos[i]>pos[i+1] 的數量即可
	    
	    > 詳細可參考 : <https://www.youtube.com/watch?v=IhMnux8RfQw>

## CSA Swap Pairing

???+note "[CS Academy - Swap Pairing](https://csacademy.com/contest/archive/task/swap_pairing/statement/)"
	給你一個長度為 $n$ 的陣列 $a_1, \ldots ,a_n$，每種數字恰出現兩次，一次操作可以 swap 相鄰的，問最少操作次數使同種數字都相鄰
	
	$n\le 10^5,n$ 為偶數 $, 0\le a_i \le 10^9$
	
	??? note "思路"
		考慮 n = 4 只會有這幾種 case :
		
		- aabb
		
		- abab
	
		- abba
	
		這啟發了我們可以把相同的字母的頭、看，看成是一個 interval，問題就變成要 interval 與 interval 倆倆之間的貢獻，例如 abcbca，那麼 interval a 對 b 的順序 abba，需要 swap 兩次才能使相同的相鄰，所以貢獻為 2，a 對 c 的順序是 acca，貢獻也是 2，b 對 c 的順序是 bcbc，因為只要 swap 一次就可使相同的相鄰，所以貢獻為 1。答案就是貢獻加總起來: 2 + 2 + 1 = 5。這個想法其實有點類似[比較排序](https://tmt514.github.io/algorithm-analysis/sorting/minimum-comparison-sort.html)
		
		最後，可以用 BIT 之類的來計算 interval 之間的貢獻，O(n log n)
		
		---
	
		考慮要使第一個數字與跟他相同的數字要相鄰，將比較後面的那個一路 swap 到第二個位置一定不會比較差，因為若他們兩個都 swap 到中間去，那等等其他人經過他們時還會花更多 cost
		
		所以我們將第一個數字做完後，就可以刪掉他們變子問題。可使用 BIT 計算 cost

## 附中模競III pE

???+note "2021 附中模競 III pE. HNO3 與音樂遊戲 (Game)"
	給兩個長度為 n 的序列 a, b，其中 a 為 1...n 的 permutation，而 b 也是 1...n 的 permutation 但可能其中一些項會變成 0。每次可以交換序列中相鄰的兩項，最少幾次操作能使所有 $b_i\neq 0$ 的項都滿足 $a_i=b_i$
	
	??? note "思路"
		考慮 b 沒有 0 的 case，我們可重新編號讓 b = [1, 2, ..., n]，問題就變逆序數對
		
		假如現在 b 有一項是 0，我們是可以直接確定那項要是什麼，然後套用上面的作法
		
		假如現在 b 有兩項是 0，可以枚舉一個放前面的，一個放後面的，然後套用上面的作法。但觀察到其實不會發生在 a 是前、後，在 b 變後、前，若發生代表他們兩個互相 swap 了，將這個操作刪掉可以得到更佳解
		
		所以歸納一下，每次交換時，交換的兩個對象應該至少一個要是 b 裡面出現的元素，不然把這個操作拔掉依舊是一個合法解。所以在最佳解的情況下不會有交換不在  b 裡的兩個元素的情形，**剩下 b 裡沒有的元素應該依照原順序填入，之後就套上面的作法**

## CSES Sorting Methods

???+note "[CSES - Sorting Methods](https://cses.fi/problemset/task/1162)"
	給一個長度為 $n$ 的陣列 $a_1, \ldots ,a_n$，以下有 4 種操作，問單獨使用每一種可讓陣列小到大 sort 的最小操作次數:

	- swap 相鄰的兩個元素
	
	- swap 任意兩個元素
	
	- 刪掉一個元素，再把它插入到另外的位置
	
	- 刪掉一個元素，再把它插入到陣列的最前面
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		> sort1
	
	    ans = 逆序數對
	
	    ---
	    
	    > sort2
	
		建邊 i → a[i]，答案為 n - cycle 數量
		
		---
		
	    > sort 3
	
	    n - LIS
		
		---
		
	    > sort4
	
	    【觀察】: 如果 $x$ 動，那小於 $x$ 的數字全部都要動（因為 $x$ 已經到最前面了，後面比她小的數字都需移到他前面才可以）
	    
	    從 $n$ 找下來看他們的順序是否從 $n$ 往左看過去遞減，答案為 n - len

## JOI 2019
???+note "[JOI 2019 p3 .有趣的家庭菜园 3](https://loj.ac/p/3012)"

    給定長度為 n 一個陣列包含 R, W, G 這三種字元，每次可以 swap 相鄰的元素，求使得一樣的字元不相鄰的最少交換次數
    
    $n\le 400$
    
    ??? note "思路"
    	我們可以發現，同一個字元之間不會有貢獻，因為他們不會互相交換。所以我們可以只存當前每個字元所放的數量即可。dp(R, G, W) 代表R 目前放的數量, G 目前放的數量, W 目前放的數量，轉移的話就枚舉最後一個放哪種字元就好
        
        $$\texttt{dp(R,G,W)}=\begin{cases} \texttt{dp(R-1,G,W) + cost} \\ \texttt{dp(R,G-1,W) + cost} \\ \texttt{dp(R,G,W-1) + cost} \end{cases}$$
    
        至於 cost 我們就預處理 query(c, i, j) 代表前 j 個種類為 c 的字元有多少個在 index i 後面。例如目前放的字元是 R，他原本在陣列裡的位置是 idx，當前狀態是 dp(i, j, k)，cost 就會是 query(G, j, idx) + query(W, k, idx)。至於我們要怎麼預處理，我們只需要存 sum(c, i) 代表前 i 位有幾個字元 c 即可，對於字元 c 的貢獻即是在 query 時回傳 max(0, j - sum(c, i)) 就可以得到。


## USCAO Dance Mooves

???+note "[USACO 2021 January 3. Dance Mooves](http://www.usaco.org/index.php?page=viewproblem2&cpid=1091)" 
	有一個長度為 $n$ 的序列 $a=[1, 2, \ldots ,n]$。給 $k$ 對指令，形式為 $(x_i,y_i)$，表示 swap$(a_{x_i},a_{y_i})$。第 $i$ 分鐘需要執行第 $i\pmod{k}$ 個動作，問 $m$ 分鐘後，每個數字分別經過幾個不同的 index。
	
	$2\le n\le 10^5, 1\le k\le 2\times 10^5, 1\le m\le 10^{18}$
	
	??? note "思路"
		令 $s_i$ 為 $i$ 在經過 $k$ 輪會走過的 unique index，$p_i$ 為 $k$ 輪後會到哪個 index。每 $k$ 輪一個循環，我們考慮建圖，邊為 $i\rightarrow p_i$，因為 in<sub>i</sub> 和 out<sub>i</sub> 都是 1，所以會形成多個 cycle。令 $m=k\times d+r$，假設某個環長度為 $L$，若 $L<d$ 則環上的每個點的答案就是環上所有 $s_i$ 的 union，反之對於某個點 $i$，只會走到以他前面的 $d$ 個，也就是 $s_i,s_{p_i},s_{p_i^2},..,s_{p_i^{d-1}}$ 的 union（其中 $p_i^x$ 為 $\underbrace{p[p[p[p...}_{x次}[i]]]]$）。因為 $\sum s_i$ 最多只會是 $2k+n$（$n$ 一開始所在的 index，$2k$ 每次 swap 會 update 兩個新的），所以對 $L<d$ 我們直接暴力的將環上的 $s_i$ 加入 ; 對於 $L \ge d$ 我們使用 sliding window 維護，這樣每個 $s_i$ 都只會加入一次和刪掉一次，總複雜度 $O(n+k)$
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	    typedef long long ll;
	
	    int N, K;
	    ll M;
	    int A[200001], B[200001];          // input
	    int P[100001];                     // as described in analysis
	    int from[100001];                  // from[i] = where the cow in position i originated from
	    vector<pair<int, int>> S[100001];  // as described in analysis, stores {pos, time}
	    int cnt[100001];                   // array to keep track of uniquePos
	    int uniquePos;                     // # of unique reachable positions
	
	    // adds in all reachable positions from S_node where time<=bar
	    void add(int node, int bar) {
	        for (auto x : S[node]) {
	            if (x.second > bar) return;
	            if (cnt[x.first] == 0)
	                uniquePos++;
	            cnt[x.first]++;
	        }
	    }
	
	    // removes all reachable positions from S_node where time<=bar
	    void remove(int node, int bar) {
	        for (auto x : S[node]) {
	            if (x.second > bar) return;
	            if (cnt[x.first] == 1)
	                uniquePos--;
	            cnt[x.first]--;
	        }
	    }
	
	    vector<int> nodes;  // stores nodes currently in cycle
	    bool vis[100001];
	
	    void dfs(int node) {
	        vis[node] = true;
	        nodes.push_back(node);
	        if (!vis[P[node]])
	            dfs(P[node]);
	    }
	
	    int main() {
	        cin >> N >> K >> M;
	        for (int i = 0; i < K; i++)
	            cin >> A[i] >> B[i];
	        // initialize from and S
	        for (int i = 1; i <= N; i++) {
	            from[i] = i;
	            S[i].push_back({i, 0});
	        }
	        // simulate the first K swaps, keeping track of where each position can reach
	        for (int i = 0; i < K; i++) {
	            S[from[A[i]]].push_back({B[i], i + 1});
	            S[from[B[i]]].push_back({A[i], i + 1});
	            swap(from[A[i]], from[B[i]]);
	        }
	        // compute array P after first K swaps
	        for (int i = 1; i <= N; i++)
	            P[from[i]] = i;
	        int ans[100001];
	        // run a DFS on each cycle
	        for (int i = 1; i <= N; i++)
	            if (!vis[i]) {
	                dfs(i);
	                ll D = M / K;   // as described in the analysis
	                int R = M % K;  // as described in the analysis
	                //"special case" if the whole cycle is included
	                if (D >= (int)nodes.size()) {
	                    D = nodes.size();
	                    R = 0;
	                }
	                int j = D - 1;
	                // initialize our sliding window [0,j]
	                for (int k = 0; k <= j; k++)
	                    add(nodes[k], K);
	                // we slide our window [i,j], adding and removing as we go
	                for (int i = 0; i < nodes.size(); i++) {
	                    int newJ = (j + 1) % (int)nodes.size();
	                    // account for the extra R swaps
	                    add(nodes[newJ], R);
	                    // store answer for current sliding window
	                    ans[nodes[i]] = uniquePos;
	                    // undo the extra R swaps
	                    remove(nodes[newJ], R);
	                    // undo the left endpoint of sliding window
	                    remove(nodes[i], K);
	                    // add new right endpoint of sliding window
	                    add(nodes[newJ], K);
	                    j = newJ;
	                }
	                // reset everything from this cycle
	                for (int k = 0; k <= D - 1; k++)
	                    remove(nodes[k], K);
	                nodes.clear();
	            }
	        for (int i = 1; i <= N; i++)
	            cout << ans[i] << endl;
	        return 0;
	    }
		```

???+note "[USACO 2020 Open Exercise G](http://www.usaco.org/index.php?page=viewproblem2&cpid=1043)"
	求所有 $k$ 的和，滿足至少存在一個 $1\ldots n$ 的 permutation 需要 $k$ 部才能回到原本的 permutation
	
	$n\le 10^4$
	
	??? note "思路"
		觀察會發會是一些環的總和要是 n，問題就變成求一些數字加起來要是 n，這些數字的 distinct lcm 總和就是答案。
		
		用 dp[i][j] 代表總合為 i，選第 1~j 個質數能湊出來的乘積總和，轉移類似背包，不同的是需要枚舉第 j 個質數用了幾次。複雜度 O(n * pi(n) * log n) 
	
	??? note "code(from 官解)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	 
	    using namespace std;
	
	    typedef long long LL;
	
	    const int MAXP = 1234;
	    const int MAXN = 10005;
	
	    LL res[MAXP][MAXN];  // result for permutations of length n restricted to using the first p primes
	
	    int n; LL m;
	
	    LL mul(LL x, LL y) {
	        return (x * y) % m;
	    }
	
	    LL add(LL x, LL y) {
	        x += y;
	        if (x >= m) x -= m;
	        return x;
	    }
	
	    LL sub(LL x, LL y) {
	        x -= y;
	        if (x < 0) x += m;
	        return x;
	    }
	
	    int main() {
	        freopen("exercise.in","r",stdin);
	        freopen("exercise.out","w",stdout);
	        cin >> n >> m;
	
	        vector<int> composite(n+1);
	        vector<int> primes;
	
	        for (int i = 2; i <= n; i++) {
	            if (!composite[i]) {
	                primes.push_back(i);
	                for (int j = 2*i; j <= n; j += i) {
	                    composite[j] = 1;
	                }
	            }
	        }
	
	        if (primes.size() == 0) {
	            cout << "1\n";
	            return 0;
	        }
	
	        for (int j = 0; j <= n; j++) res[0][j] = 1;  // identities
	
	        for (int i = 1; i <= primes.size(); i++) {
	            for (int j = 0; j <= n; j++) {
	                res[i][j] = res[i-1][j];
	
	                int pp = primes[i-1];
	                while (pp <= j) {
	                    res[i][j] = add(res[i][j], mul(pp, res[i-1][j-pp]));
	                    pp *= primes[i-1];
	                }
	            }
	        }
	
	        cout << res[primes.size()][n] << "\n";
	    }
		```

???+note "[JOI 2021 Final 集合写真](https://www.luogu.com.cn/problem/P7406)"
	給一個 $1\ldots n$ 的 permutation $a$，每次可以 swap 相鄰項，問最少幾次操作可滿足:

    - 對於任意 $i\in[i,n-1]$ 都有 $a_i<a_{i+1}+2$
    
    $3\le n\le 5000$
    
    ??? note "思路"
    	可以發現最後的序列一定可以分成若干段連續遞減的子序列，而且前面的子序列與後面子序列是從小到大的，例如 $[3,2,1,6,5,4,9,8,7]$
    
        所以我們可以令 $dp(i,\ell)=$ 由 $1\ldots i$ 組成的合法序列，**最後一段長度為 $\ell$** 的最小 swap 次數。序列大概就是這樣:
        
        $$
        1\ldots i-\ell,\space i,\space i-1\ldots i-\ell + 1
        $$
        
        考慮 $i$ 對序列給出的貢獻，我們先列出轉移式 $dp(i,\ell)=dp(i-1,\ell-1)+\text{cost}$，也就是將 $i$ 插入在中間，那麼 $i$ 的貢獻就是:
    
        - 原序列 $i$ 後面有多少個 $\le i-\ell$
    
        - 加上 $i$ 前面有多少個 $\in [i-\ell + 1, i-1]$
    
        這可以將 $(i,a_i)$ 打在二維平面上，用二維前綴和預處理得到，複雜度 $O(n^2)$

https://drive.google.com/file/d/1WB9Hnx3itjsBQO0Qk06e1Iszt7jNzhVF/view
