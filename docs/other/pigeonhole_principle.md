???+note "[2023 YTP p6. 胖胖貓的減肥計畫 (Weight_Loss_Plan)](https://yozen0405.github.io/wiki/basic/brute_force/images/YTP2023FinalContest_S2_TW.pdf)"
	
???+note "[2023 成大賽 pF. 動物園](https://codeforces.com/gym/437848/problem/F)"
	有一個無限項的序列 $s$，其中 $s_i=12345\ldots i$。有 $t$ 筆查詢，每筆查詢給 $x$，問是否存在兩個 $i,j$ 滿足 $s_i\% x$ == $s_j\% x$

	$1\le t\le 10^6, 1\le x\le 10^6,\sum x \le 2\times 10^6$
	
	??? note "思路"
		根據鴿籠原理，只要看任意 x + 1 個東西，一定會有兩個東西 % x 的結果是相同的
		
		所以我們只要將 s[1], ..., s[x + 1] 算出來輸出 % x 一樣的兩個即可
	
???+note "[CF 961 D. Pair Of Lines](https://codeforces.com/problemset/problem/961/D)"
	
---

- <https://zh.wikipedia.org/zh-tw/%E9%B4%BF%E5%B7%A2%E5%8E%9F%E7%90%86>