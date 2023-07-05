## 交換法

- sorting [APCS 物品堆疊](https://zerojudge.tw/ShowProblem?problemid=c471) 
- merge sort [CF 559B](https://codeforces.com/problemset/problem/559/B)

## 霍夫曼編碼

???+note "例題"
	- 給你一個陣列 $a$ , 合併 $a_x,a_y$ 要花 $a_x+a_y$ ( $x,y$ 不一定要相鄰)

 	- 求把陣列整個合併完得最小花費

??? note "思路"
    - 貪心的想法每次都合併最小的
    - 塞進 $\texttt{pq}$ 每次去最小的 $\texttt{a,b, pop}$ 掉再 $\texttt{push(a + b)}$ 

