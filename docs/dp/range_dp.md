???+note "[CF 1025 D. Recovering BST](https://codeforces.com/problemset/problem/1025/D)"
	給一個長度為 $n$ 的中序陣列 $a_1,\ldots ,a_n$，問是否能組出一顆 BST 滿足相鄰兩點的 $\gcd (a_i, a_j)=1$
	
	$2\le n\le 700, 2\le a_i \le 10^9$
	
	??? note "思路"
		令 L(i, j) 為以 j 為根，[i, j - 1] 當左子樹是否能組出一顆合法 BST
		
		令 R(i, j) 為以 i 為根，[i + 1, j] 當右子樹是否能組出一顆合法 BST
		
		對於一個區間 [l, r]，枚舉 k 當 [l, r] 的根（用 L(l, k) && R(k, r) 判斷），看看是 k 否能接到 l - 1 的右子樹或 r + 1 的左子樹，可行則分別設 R(l - 1, k) = true，或 L(k, r + 1) = true
		
		可以先預處理 G(i, j) 代表 i 是否能根 j 連邊，在轉移時維護一個 ans(l, r) 代表 [l, r] 是否能組出一顆合法的 BST，最後輸出 ans(l, r) 即可

???+note "[全國賽模擬賽 pF. 鬧鐘設置 (F_Alarm_Clock)](https://drive.google.com/file/d/1KAaPOYdzBuoC0hXdFwS5W0ZEItY61ktk/view)"
	
	
???+note "[JOI 2015 Final 分蛋糕 2](https://loj.ac/p/2725)"

array shrinking

atcoder dp contest slimes

2023 toi 2, pB