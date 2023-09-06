- LIS, LDS, Dilworth's theorom
- https://zerojudge.tw/ShowProblem?problemid=b903
- sprout oj 421
- TOI 2021 pC
- https://www.luogu.com.cn/training/215

$$dp[i]=\max \limits_{j< i \texttt{ and }a_j<a_i} \{ dp[j] + 1 \}$$

LIS 的圖 : (將 $(i,a_i)$ 打在二維座標上看)



<figure markdown>
  ![Image title](./images/7.png){ width="600" }
  <figcaption>Dilworth's theorom</figcaption>
</figure>

???+note "[Atcoder arc149 B. Two LIS Sum](https://atcoder.jp/contests/arc149/tasks/arc149_b)"
	給你兩個 $1\ldots n$ 的 permutation $a, b$，你可以做以下操作任意次
	
	- 選一個 $i$，swap$(a_i, a_{i + 1})$ 並 swap$(b_i, b_{i + 1})$

	輸出 LIS$(a)+$ LIS$(b)$ 最大可以是多少
	
	$2\le n\le 3\times 10^5$
	
	??? note "思路"
		我們考慮最後的答案 LIS 選擇的東西會是什麼，如果有沒選的，讓 a 把他選起來一定更好
		<figure markdown>
          ![Image title](./images/13.png){ width="500" }
        </figure>
		
		如果 b 有選 a 沒選的，讓 a 選答案一定不會變差
		
		<figure markdown>
          ![Image title](./images/14.png){ width="500" }
        </figure>
        
        所以我們可以得出一個結論 : 先將 a sort 好（b 也跟著 a 的順序變），答案就是 |a| + LIS(b)
	