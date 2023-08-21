LOJ 題單

## 序列分塊

一次好幾塊的 O(n / k)，暴力 jump，lazy tag

一塊的 O(k)，暴力修改


## 值域分塊

???+note "靜態區間第 k 小"
	
## 操作分塊

在同一個 block 裡的，暴力掃過，複雜度 O(Q * k)

在 block 外的，要讓他單調的進來（整個 block 最多只會 O(n)），複雜度 O(n * (n / k))

???+note "[LOJ #6280. 数列分块入门 4](https://loj.ac/p/6280)"
	給一個長度為 $n$ 的陣列 $a$，有 $q$ 筆操作 :
	
	- 區間加值
	
	- 區間求和
	
	$1\le n\le 5\times 10^4$
	
	??? note "思路"
		將操作依照時間小到大分塊，每 k 個一組。對於每個 query(ql, qr)，暴力掃過跑該 block 裡面所有的 add(l, r)，計算 [l, r] 在 [ql, qr] 的貢獻。對於每個 block 結束後再重新執行一次前綴和，複雜度 O(q * k + (n / k) * n)

???+note "[APIO2019 桥梁](https://loj.ac/p/3145)"
	給定一張 n 個點 m 邊的無向圖和 q 次詢問。可以：  
	
	1. 修改某條邊的邊權
	
	2. 從點 u 出發，只經過邊權不小於等於 k 的邊，可以到幾個點
	
	$1\le n\le 5\times 10^4,0\le m\le 10^5,1\le q\le 10^5$
	
	??? note "思路"	
		將操作依照時間小到大分塊，每 k 個一組。每組將裡面的 query 從大到小處理，對於每一個 query，依序加入非修改的邊，有修改的邊就直接全部暴力掃過，掃完之後要到下一個 query 的時候需要 rollback。
		
		非修改邊在一個 block 中最多掃到 m 個，共 O(m * (q / k))，修改的邊 O(q * k)，還要乘上 rollback dsu 的 O(log n)，複雜度 O(m * (q / k) * logn + q * k * log n) 

## 數論分塊

???+note "[Zerojudge d193. 11526 - H(n)](https://zerojudge.tw/ShowProblem?problemid=d193)"
	給定 $1\le n,k\le 10^9$，求
	
	$$
	\sum \limits_{x=1}^k \lfloor\frac{k}{x}\rfloor
	$$
	
	??? note "思路"
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	
	    using namespace std;
	
	    void solve() {
	        int n;
	        cin >> n;
	
	        int total = 0, at = 1;
	        while (at <= n) {
	            int cur = n / at;  // n / i 到多少了
	            int last_same = n / cur; // 這個 n/i 的質最多可以延續到哪格
	
	            total += cur * (last_same - at + 1);
	            at = last_same + 1;
	        }
	
	        cout << total << endl;
	    }
	
	    signed main() {
	        int t;
	        cin >> t;
	        while (t--) {
	            solve();
	        }
	    }
		```
		
???+note "[CSES - Sum of Divisors](https://cses.fi/problemset/task/1082)"
	令 $\sigma(n)$ 為 $n$ 的因數相加總和，問 $\sum \limits_{i=1}^n \sigma(n)$
	
	$1\le n\le 10^{12}$
	
	??? note "思路"
	    觀察 12
	
		<figure markdown>
	      ![Image title](./images/13.png){ width="300" }
	    </figure>
	
	    每個數字出現 $\frac{n}{i}$ 次，答案就是 $\sum\frac{n}{i}\times i$ 其中 $i=1...n$
	
		<figure markdown>
	      ![Image title](./images/14.png){ width="300" }
	    </figure>
		
	    就變成 zerojudge - H(n) 的題目了
	
	??? note "code"
	    ```cpp linenums="1"
	    #include <iostream>
	
	    using std::cout;
	    using std::endl;
	
	    const int MOD = 1e9 + 7;
	
	    int main() {
	        long long n;
	        std::cin >> n;
	
	        long long total = 0;
	        long long at = 1;
	        while (at <= n) {
	            long long add_amt = n / at;  // n / i 到多少了
	            long long last_same = n / add_amt; // 這個 n/i 的質最多可以延續到哪格
	
	            total = (total + add_amt * (last_same - at + 1));
	            at = last_same + 1;
	        }
	
	        cout << total << endl;
	    }
	    ```
---

## 資料

- <https://zhuanlan.zhihu.com/p/452977429>