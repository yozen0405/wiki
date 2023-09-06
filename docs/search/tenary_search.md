

???+note "[CS Academy Growing Trees](https://csacademy.com/contest/archive/task/growing-trees)"
	給一棵 $n$ 個點的樹，在第 $k$ 天的時候第 $i$ 條邊的權重是 $C_i + k \times A_i$，求在第 $[0, D]$ 天內，什麼時候會有最短的樹直徑
	

	$n\le 2.5 \times 10^5,0\le D\le 10^6$
	
	??? note "思路"
		觀察到 $C_i + k \times A_i$ 會是一個峰函數，可以三分搜極值

- 凸函數，凹函數，定義
- <https://codeforces.com/problemset?tags=ternary%20search>
- 201B. Guess That Car!
- 578C. Weakness and Poorness
- 1716C. Robot in a Hallway
- 1265B2. Send Boxes to Alice (Hard Version)