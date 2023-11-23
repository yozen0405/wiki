## 環狀最大連續子序列

???+note "[LeetCode 918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)"
	給一個長度為 $n$ 的環形陣列 $a_1, \ldots ,a_n$，問最大連續子陣列，不能到選空的
	
	$n\le 3\times 10^4, -3\times 10^4\le a_i\le 3\times 10^4$

<figure markdown>
  ![Image title](./images/10.png){ width="500" }
</figure>

$$ans=\max \begin{cases} \texttt{max subarray} \\ \sum a_i-\texttt{min subarray}\end{cases}$$

不能選到空的 ⇒ 當 ans = 0 時，回傳陣列中最大的元素

proof

$$\begin{align} \max(\texttt{pre} + \texttt{suf}) &= \max(\texttt{total sum} - \texttt{subarray})
\\ &= \texttt{total sum} + \max(-\texttt{subarray})
 \\ &= \texttt{total sum} - \min(\texttt{subarray}) \end{align}$$

## 練習題

???+note "[2022 nhspc-mock pB](https://www.csie.ntu.edu.tw/~b11902109/PreNHSPC2022/IqwxCSqc_Pre_NHSPC_zh_TW.pdf)"
	給 n 個陣列 $a_1, \ldots ,a_n$，問這些陣列以任意順序組合起來 Maximum Subarray Sum 最大是多少
	
	$n\le 10^5, \sum |a_i| \le 10^6, |a_{i, j}| \le 10^9$
	
	??? note "思路"
		先想 O(n^2) 怎麼做，我們可以預處理每個陣列的前綴最大值 pre[i]，後綴最大值 suf[i]，然後枚舉左右，但這時，我們要怎麼計算中間的貢獻 ? 我們令 sum[i] = max(陣列 i 的總和, 0)，當左為 l, 右為 r 時，答案就是 
		
		<center>
		suf[l] + (全部的總和 - sum[l] - sum[r]) + pre[r]
		</center>
		
		那我們要怎麼做得更快呢 ? 我們考慮只枚舉當左右的其中一個，另一個看能不能快速的找出來。可以發現，若我們枚舉 l，那麼我們就只要找到 (pre[r] - sum[r]) 最大的即可（將上面的試子整理一下可推得）。所以我們將所有陣列的 (pre[i] - sum[i]) sort 好後，對一個 l 只要去看最大與次大（因為最大的可能就是 l）的 (pre[i] - sum[i]) 即可
	 
???+note "[zerojudge i402. 4. 內積](https://zerojudge.tw/ShowProblem?problemid=i402)"
