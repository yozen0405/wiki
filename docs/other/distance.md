## 介紹

- 歐幾里德：$\sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$

- 曼哈頓（計程車幾何）：$|x_i - x_j| + |y_i - y_j|$

- 切比雪夫：$\max(|x_i - x_j|, |y_i - y_j|)$


## 轉換

先來看圖

<figure markdown>
  ![Image title](./images/2.png){ width="100" }
</figure>

### 曼哈頓 ⇒ 切比雪夫

$$(x,y)\Rightarrow (x+y,x-y)$$

原本座標的曼哈頓距離 = 新座標中的切比雪夫距離

### 切比雪夫 ⇒ 曼哈頓

$$(x,y)\Rightarrow (\frac{x+y}{2},{x-y}{2})$$

原本座標的切比雪夫距離 = 新座標中的曼哈頓距離 

???+note "[IOI 2007 Pairs](https://tioj.ck.tp.edu.tw/problems/1345)"
	有一個盤面大小為 $m$，給 $n$ 個 $B$ 座標點 $p_i$，問有幾對 $(i,j)$ 使曼哈頓距離 $dis(p_i,p_j)$ 不超過 $D$
	
	$B\in \{1,2,3\}, n\le 10^5,m\le \{7.5\times 10^7, 7.5\times 10^4, 75\}$
	
	??? note "思路"
		一維: two pointer
		
		二維: sweep line
		
		- (x-d, y) ⇒ +1

		- (x, y) ⇒ query[y-d, y+d]

		- (x+d, y) ⇒ -1

		三維: m^2 枚舉 z，變二維的問題，注意自己會算到自己，所以答案要減 n
		
![](https://scontent.ftpe3-2.fna.fbcdn.net/v/t1.15752-9/333243039_907232120429533_8752551099725427151_n.jpg?_nc_cat=103&ccb=1-7&_nc_sid=ae9488&_nc_ohc=0OcBwAbY8rEAX-7hBn6&_nc_ht=scontent.ftpe3-2.fna&oh=03_AdRm051Mbt0PfYsP_kCF0DCh7IybwJSl_sWuHHndimmpbQ&oe=64ABBAF9)

---

- <https://hackmd.io/@FHVirus/SJ0kzMGM_#/9/5>
