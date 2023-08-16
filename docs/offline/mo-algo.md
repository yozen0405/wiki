## 介紹

### 將 query 排序

分成 sqrt(n) 個 block

每筆 Query 按照 : 

1. 左界所屬 Block 從小排至大

2. 若左界 block 相同，則將右界從小排至大

???+note "code"
	```cpp linenums="1"
	struct Query {
        int l, r, block_id;

        bool operator<(const Query &rhs) const {
            if (block_id != rhs.block_id) {
                return block_id < rhs.block_id;
            } else {
                return r < rhs.r;
            }
        }
    };
    ```

### 暴力移動 pointer

注意先擴大（add），再縮小（del）[^1]

???+note "code"
	```cpp linenums="1"
	int l = -1, r = 0;
	for (auto &i : Query) {
		while (l > i.l) add(--l);
		while (r > i.r) add(++r);
		while (l < i.l) del(l++);
		while (r > i.r) del(r--);
	}
	```

### 維護 data structure

- O(1) 新增/刪除

- O(sqrt(n)) 查詢

    - 總和
    - x 出現的次數
    - mode
    - 第 k 小

### 複雜度

- left pointer

    - 同塊 : 同個 block 裡面 l 最多移動 sqrt(n) 格，O(Q * sqrt(n))
    - 不同 : 不同 block 之間的總移動距離為 O(N)

- right pointer

    - 同塊 : 每個 block 裡面 r 最多會移動 N 格，共 sqrt(N) 個 block，O(N * sqrt(N))
    - 不同 : 換 block 最多 sqrt(N) 次，每次最多移動 N 格，O(N * sqrt(N))

每 k 個當一個 block :

- left: 同個 block 每次移動 k，共 Q 次 ⇒ O(Q * k)

- right: N / k 個 block，每個 block O(N) ⇒ O(N * (N / k))

## 題目

把問題轉換換成 add, del, query，其中讓 add, del 快，query 慢

???+note "[CF 86 D. Powerful Array](https://codeforces.com/problemset/problem/86/D)"

	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，有 $q$ 筆詢問 :
	
	- 給一個區間 $[l,r]$，將區間內每種數字與其出現的次數平方的乘積加總後輸出
	
	$n,q\le 2\times 10^5,1\le a_i \le 10^6$
	
	??? note "思路"

???+note "[Zerojudge b417. 區間眾數](https://zerojudge.tw/ShowProblem?problemid=b417)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，$q$ 次詢問一個區間的 :
	
	- 眾數出現的次數
	
	- 多少種數字可當眾數
	
	$n\le 10^5,q\le 10^6$
	
	??? note "思路"
		- freq[i] : i 出現次數
	
	    - cnt[i] : 幾種數字的出現次數為 i
	
	    - mode : 眾數的出現次數
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pii pair<int, int>
	    #define mk make_pair
	    #define pb push_back
	    using namespace std;
	
	    const int maxn = 1e5 + 5;
	    int cnt[maxn], freq[maxn], a[maxn], mode;
	    int n, q;
	
	    struct Query {
	        int l, r, block_id, query_id;
	
	        bool operator<(const Query &rhs) const {
	            if (block_id != rhs.block_id) {
	                return block_id < rhs.block_id;
	            } else {
	                return r < rhs.r;
	            }
	        }
	    };
	
	    void add(int x) {
	        cnt[freq[x]]--;
	        freq[x]++;
	        cnt[freq[x]]++;
	        if (freq[x] > mode) mode = freq[x];
	    }
	
	    void del(int x) {
	        cnt[freq[x]]--;
	        freq[x]--;
	        cnt[freq[x]]++;
	        if (cnt[mode] == 0) mode--;
	    }
	
	    signed main() {
	        cin >> n >> q;
	        vector<Query> query;
	        for (int i = 0; i < n; i++) {
	            cin >> a[i];
	        }
	        int lb, rb;
	        int k = sqrt(n);
	        for (int i = 0; i < q; i++) {
	            cin >> lb >> rb;
	            lb--, rb--;
	            query.pb({lb, rb, lb / k, i});
	        }
	        sort(query.begin(), query.end());
	
	        int l = 0, r = -1;
	        vector<pii> ans(q);
	        for (int i = 0; i < q; i++) {
	            Query now = query[i];
	
	            while (l > now.l) add(a[--l]);
	            while (r < now.r) add(a[++r]);
	            while (l < now.l) del(a[l++]);
	            while (r > now.r) del(a[r--]);
	
	            ans[now.query_id] = {mode, cnt[mode]};
	        } 
	
	        for (auto &p : ans) {
	            cout << p.first << " " << p.second << "\n";
	        }
	    }
	    ```

???+note "[LOJ #6285. 数列分块入门 9](https://loj.ac/p/6285)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，$n$ 次詢問一個區間的最小眾數

	$n\le 10^5$
	
	??? note "思路"
		離散化，預處理 dp[i][j] = block(i) ~ block(j) 的眾數，可以枚舉 i 然後 O(n) 做下去，時間複雜度 O(n * sqrt(n))，當預到一個 query(l, r) 時，可以分成 l ~ r 之間的完整 block，與左右不完整的 block 來做，完整 block 直接查表，不完整 block 直接暴力跑即可，過程中要檢查 l r 之間 x 出現的次數可用 vector[x] 存 x 出現的 index 然後去 lower bound，複雜度 O(q * sqrt(n) * log n)
		
		> 參考 : <https://blog.csdn.net/hypHuangYanPing/article/details/81260095>

???+note "[LOJ #6762. 「THUPC 2021」本质不同逆序对](https://loj.ac/p/6762)"
	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，$q$ 次詢問一個區間的逆序數對數量

	$n\le 10^5,m\le 5\times 10^5$

CSES - Distinct Value Query (多種作法)
TIOJ 1699 Problem I 害蟲決戰時刻
atcoder ABC 174 F
codeforces 86 D

## 帶修改莫隊

???+note "[CF 940 F. Machine Learning](https://codeforces.com/problemset/problem/940/F)"
	給一個長度為 $n$ 個陣列 $a_1,\ldots ,a_n$，有以下 $q$ 個操作 :
	
	- $\text{mex}(l,r):$ 輸出區間數字出現次數的 mex
	
	- $\text{update}(i,x):$ 將 $a_i$ 改成 $x$
	
	$n,q\le 10^5,1\le a_i \le 10^9$



## 回滾莫隊

???+note "[JOISC 2014 Day1 历史研究](https://loj.ac/p/2874)"
	
	
???+note "[CF edu dsu B. Number of Connected Components on Segments](https://codeforces.com/edu/course/2/lesson/7/3/practice/contest/289392/problem/B)"
	

---

## 參考資料

- <https://sam571128.codes/2020/10/03/MO-Algorithm/>

- <https://cp.wiwiho.me/mo-algorithm/>

- <https://docs.google.com/document/d/11-Ho9_nnds76VdfBfDQC7U1sYjXVrlNgEurdZ4ykL4s/edit#heading=h.isbo4ewxeenw>

- <https://drive.google.com/file/d/1F73WYrDtwoH_VtjYK4qicKDuEFDu9OqY/view>

- <https://hackmd.io/@iceylemon157/HkdBTBJEK>

- <https://www.cnblogs.com/RioTian/p/15113195.html>

[^1]: [2, 5] → [6, 7] 如果先移動左邊，可能會變成 [6, 5]，無法保證左界小於等於右界