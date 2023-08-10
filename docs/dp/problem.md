???+note "[2023 TOI 4 模 p2. 整除序列 (Divide)](https://drive.google.com/file/d/1CvB1-JyGmFaKq1Ir8VJ5avx7NUFT2Hl9/view)"
	有一個長度為 $n$ 的陣列 $a_1,\ldots, a_n$，你只知道每個數字的其中一個 character，要構造出陣列，使得相鄰的兩個數字互為因數或倍數關係，且補上的 character 數量要越少越好
	
	$2\le n\le 10^5$
	
	??? note "思路"
		可以觀察到每一項的數字都不會太大，所以可以先猜一個 threshold T
		
		然後去 dp(i, x) : 0~i 要讓 a[i] = x 最少要填的 character 數量
		
		f(i, k) : 0~i 要讓 a[i]|k 或 k|a[i] 最少要填的 character 數量
		
		f(i, k) = min{dp(i, x) + size(x) - 1} 其中 x|k or k|x 且 dp(i, x) 有解
		
		dp(i + 1, k) = 
		
		- k 有 b[i + 1]: f(i, k) + size(k) - 1

		- otherwise: -1

		可以先預處理 0~T 每個數字會不會出現某個 character exist[x][0~9]。這樣對於每個 i 都要做一次篩法從 dp(i, x) 轉移到 f(i, k)，複雜度 O(n * T * log(T))。