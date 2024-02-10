???+note "[USACO 2021 December Contest, Silver Problem 3. Convoluted Intervals](https://www.usaco.org/index.php?page=viewproblem2&cpid=1160)"
	給 $n$ 個 intervals $(a_i,b_i)$。問對於 $k=0\sim 2m$ 的每個 $k$ 問有多少 pair $(i,j)$ 滿足 $a_i + a_j \leq k \leq b_i + b_j$
	
	$n\le 2\times 10^5,m\le 5000,0\le a_i,b_i \le m$
	
	??? note "思路"
		因為 $m$ 只有 $5000$，我們將開一個大小為 $m$ 的 vector，第 $x$ 格存 $a_i=x$ 的 $i$ 的數量，直接 $m^2$ 枚舉 vector 裡面的 index
		
		> 參考自 : <https://www.usaco.org/current/data/sol_prob3_silver_dec21.html>
		
	??? note "code(from usaco)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    using namespace std;
	
	    int main() {
	        cin.tie(0)->sync_with_stdio(0);
	        int N, M;
	        cin >> N >> M;
	        vector<pair<int, int>> ivals(N);
	        for (auto &ival : ivals)
	            cin >> ival.first >> ival.second;
	        vector<int64_t> win_start(2 * M + 1), win_end(2 * M + 1);
	        {
	            vector<int64_t> a_freq(M + 1);
	            for (int i = 0; i < N; ++i)
	                ++a_freq.at(ivals.at(i).first);
	            for (int i = 0; i <= M; ++i)
	                for (int j = 0; j <= M; ++j)
	                    win_start.at(i + j) += a_freq.at(i) * a_freq.at(j);
	        }
	        {
	            vector<int64_t> b_freq(M + 1);
	            for (int i = 0; i < N; ++i)
	                ++b_freq.at(ivals.at(i).second);
	            for (int i = 0; i <= M; ++i)
	                for (int j = 0; j <= M; ++j)
	                    win_end.at(i + j) += b_freq.at(i) * b_freq.at(j);
	        }
	        int64_t win_count = 0;
	        for (int i = 0; i <= 2 * M; ++i) {
	            win_count += win_start.at(i);
	            cout << win_count << "\n";
	            win_count -= win_end.at(i);
	        }
	    }
		```

???+note "[CS Academy - Candles](https://csacademy.com/contest/archive/task/candles/statement/)"
	給一個長度為 $n$ 的陣列 $a_1, ... ,a_n$ ，依序有 $m$ 天，第 $i$ 天要選 $c_i$ 個數字減一，問最多能做能持續幾天

	$n, m \le 10^5$
	
	??? note "思路"
		greedy 策略，拿最大的 $c_i$ 個出來減一。
	
	    先找到目前第 $c_i$ 大的數字 $x$ 
	
	    - 大於 $x$ 的所有數字都直接減一
	
	    - 假設數字 $x$ 要刪除的數量是 $t$，找到最左邊的 $t$ 個 $x$ 拿出來減一
	
		這可以用線段樹來解決

???+note "[CF 1919 F1. Wine Factory (Easy Version)](https://www.luogu.com.cn/problem/CF1919F1)"
	有座水塔，其中第 $i$ 座包含 $a_i$ 公升的水，且擁有一個能消除 $b_i$ 公升水的法師。同時，相鄰兩座水塔之間擁有一道閥門，第 $i$ 座高塔與第 $i+1$ 座水塔之間的閥門允許通過 $c_i$ 升水。

    對於第 $i=1,2,\ldots ,n$ 座塔，將會進行以下操作：

    1. 法師將水塔 $i$ 中最多 $b_i$ 公升的水消除，並將消除的水轉化為等量的紅酒。
    2. 如果 $i \neq n$，那麼至多 $c_i$ 公升水可以由閥門流到第 $i+1$ 座水塔。

    有 $q$ 次單點修改，每次給出四個數字 $p,x,y,z$，即將 $a_p = x, b_p = y, c_p = z$，並要求對於每次更新，輸出最多能獲得多少公升紅酒。

    $n\le 5\times 10^5, a_i \le 10^9, c_i = 10^{18}, z = 10^{18}$
    
    ??? note "思路"
    	因為 $c_i, z$ 均為 $10^{18}$，那麼如果水塔中的水沒有被全部消除，則剩下的水均能流到下一座高塔，即可以不考慮 $c_i$ 這個限制因素。
    	
    	先考慮每座水塔本身無法消除的水量：$v_i=a_i-b_i$，當 $v_i > 0$ 時，未被消除的水量必然需要由後面的法師來進行消除（如果可以的話），因此對於水塔 $k$ 而言，無法消除的水量實際上為 $f(k)=\sum \limits_{i=k}^n v_i$。那麼對於所有水塔來說，無法消除的水量即為 $\max \{ f(1), f(2), \ldots ,f(n) \}$，因為前面的無法消除後面的，所以一旦某個後綴 $f(i)$ 很大時，那些水就會被留在這個水桶 $i$。
		
		使用後綴和建造一棵線段樹，維護區間上的最大值，並使用變數 $\text{sum}$ 記錄水塔的總水量。
		
		每次更新水塔 $p$ 時，由於線段樹維護的是後綴和，那麼對於所有 $(p+1)\sim n$ 上的點，都不會收到影響，只需要對區間 $[1, p]$ 進行更新，先減去原本的 $v_p$，再加上目前更新後的新 $v_p = x - y$。
		
		那麼最後的答案就是 $\text{sum} - \max \{ f(1), f(2), \ldots ,f(n) \}$，其中 $\max \{ f(1), f(2), \ldots ,f(n) \}$ 為線段樹上的訊息。
	