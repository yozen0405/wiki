- [貪心 - APCSC](/wiki/math/special/images/1.pdf)
- https://atcoder.jp/contests/abc236/tasks/abc236_e

### 平均最大化

???+note "[P1570 KC 喝咖啡](P1570 KC 喝咖啡)"
	$N$ 個物品重量與價值分別為 $w_i$, $v_i$，選 $K$ 個，求單位重量的價值最大多少
	
	??? note "思路"
		$$\begin{align}& \dfrac{\sum v_i}{\sum w_i}\ge x \\ \\ \Rightarrow & \sum v_i \ge \sum(x\times w_i) \\ \\  \Rightarrow & \sum (v_i - x\times w_i) \ge 0 \end{align}$$
		
		二分搜 $x$，選 $(v_i - x\times w_i)$ 最大的 $K$ 個判斷
		
	??? note "code"
		```cpp linenums="1"
		bool check (double x) {
	        for (int i = 0; i < n; i++) {
	            c[i] = w[i]- x * v[i];
	        }
	        sort(c, c + n,greater<double>());
	        
	        float sum = 0;
	        for (int i = 0; i < k; i++) {
	            sum += c[i];
	        }
	        
	        return sum >= 0;
	    }
	
	    while (R - L > 0.0001) {
	        double m = (L + R) / 2;
	        if (check (m)) {
	            L = m;
	        } else {
	            R = m;
	        }
	    }
	    ```



### 區間平均
???+note "[P1404 平均数](https://www.luogu.com.cn/problem/P1404)"
    給定 $a_1,...,a_n$ 問可否選出 $a_l,..., a_r$ 使得平均最大，且長度要 $\ge m$
    
    - $1\le n,m \le 10^5$
    
    ??? note "思路"
    	二分搜平均值 $x$
    	
    	$\texttt{check} (x)$ 是檢查有沒有平均大於等於 $x$ 的
    	
		也就是看 $pre(i) - pre(j) \ge 0$

### 區間平均
???+note "區間平均"
	給定 $a_1,...,a_n$ 問可否選出 $a_l,..., a_r$ 使得平均為 $x$
    
    ??? note "思路"
        將 $a_i$  的變成 $a_i - x$
        
        問題就變成取 $pre_i- pre_j = 0$