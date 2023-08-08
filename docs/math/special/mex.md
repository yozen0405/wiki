## 介紹

$\text{mex}(S)$ : 回傳最小沒有出現在集合 $S$ 的非負整數

例如 : 

- $\text{mex}( \{1, 2, 4\} ) = 0$

- $\text{mex}( \{0, 1, 2, 4\} ) = 3$

- $\text{mex}( \{0, 1, 2, 3\} ) = 4$

???+note "code"
	```cpp linenums="1"
	int mex(vector<int>& a) {
        int n = a.size();

        vector<bool> v(n + 1, false);
        for (int x : a) {
            if (x <= n) v[x] = true;
        }

        for (int i = 0; i <= n; i++) {
            if (v[i] == false) return i;
        }
        return -1;
    }
    ```

## 例題

???+note "[CSES - Missing Number](https://cses.fi/problemset/task/1083)"

???+note "[洛谷 P4137](https://www.luogu.com.cn/problem/P4137)"

???+note "[IOIC p.52]()"

???+note "[ionc區間mex]()"

???+note "[2022 花中一模 pE]()"
	
???+note "[Atcoder abc272 E. Add and Mex](https://atcoder.jp/contests/abc272/tasks/abc272_e)"
	
	
???+note "[CF 1844 B. Permutations & Primes](https://codeforces.com/contest/1844/problem/B)"
	問 $1,2,\ldots ,n$ 的 permutation 裡面，最多能有幾個區間裡面的數自 $\text{MEX}$ 起來是質數，輸出這個 permutation
	
	$n\le 2\times 10^5$
	
	??? note "思路"
		首先觀察每個位置會有幾個區間 $(l,r)$ 覆蓋到，以 $n=5$ 為例，每個 $cnt_i=$左邊的數字個數 $\times$ 右邊的數字個數 :
		
		$$cnt=[5,8,9,8,5]$$
		
		如果一個 subarray 裡面沒有 $1$ 那 $\text{MEX}$ 就是 $1$，所以 $1$ 需要放在最多個區間 $(l,r)$ 會覆蓋到的位置，也就是最中間
		
		顯然 $\text{MEX}$ 要是越大的數字越難達到，那不如我們就先考慮比 $1$ 大一點的質數 $2$。我們想要使盡量少的區間覆蓋到 $2$，使得盡量多的區間 $\text{MEX}$ 是 $2$，那麼放在最邊邊一定是最好的，不失一般性假設 $2$ 是放在最前面。
		
		我們來整理一下目前的區間包含 $1,2$ 的情況
		
		1. 沒有 $1$，有 $2$ $\Rightarrow \text{MEX}=1$
		
		2. 沒有 $1$，沒有 $2$ $\Rightarrow \text{MEX}=1$
	
		3. 有 $1$，沒有 $2$ $\Rightarrow \text{MEX}=2$
	
		4. 有 $1$，有 $2$ $\Rightarrow \text{MEX} > 2$
	
		發現前三種情況的 $\text{MEX}$ 已經固定，所以我們要想辦法使第四種的 $\text{MEX}$ 是質數的區間個數盡量多。有 $1$，有 $2$ 的區間一定是 $l=1,r\ge \text{mid}$，而 $3$ 是大於 $2$ 的第一個質數，也就是 $\text{MEX}$ 最好達到的質數，所以我們要使 $l=1,r\ge \text{mid}$ 的區間包含 $3$ 的個數要越少越好，那當然就是把 $3$ 放在陣列的尾端。
	
		實作上將 $2$ 放頭，$3$ 放尾，$1$ 置中，其他數字隨便放，就樣就完成此題了

---

## 資料

- [CSDN Blog](https://blog.csdn.net/jokercold/article/details/78778434?ops_request_misc=&request_id=&biz_id=102&utm_term=mex%20%E5%8C%BA%E9%97%B4%E5%86%85%E6%9C%AA%E5%87%BA%E7%8E%B0%E7%9A%84%E6%9C%80%E5%B0%8F%E6%AD%A3%E6%95%B4%E6%95%B0&utm_medium=distribute.wap_search_result.none-task-blog-2~all~sobaiduweb~default-2-.wap_first_rank_v2_rank_v29&spm=1018.2118.3001.4187)