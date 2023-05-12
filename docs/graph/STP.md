補: TOI 2023 一模 pD

## Dijkstra

單源點最短路徑

考慮不帶權的單點源最短路，我們用BFS維護一個 queue，每次處理一個點時最短路徑大小已知，因此只需要拿該點去**更新其他點一次**在**沒有負邊**的假設下，Dijkstra 就像是有帶權的BFS。

### 證明

補: 證明

補: 實作

### 多源點 dijkstra

??? "例題 (ZJ ???)"
	給一張無向圖，與 $k$ 個源點，求這些點兩兩之間的距離最小值
	
最暴力的想法就是枚舉源點，每次都重跑 dijkstra，複雜度 O(k E log⁡E )

從上面暴力的方法我們可以觀察出，要交會的點或邊一定要是源點們之間的最短路。

每個源點能擴展出他能控制的最短路徑區域，如下圖

<figure markdown>
  ![Image title](./images/5.png){ width="300" }
  <figcaption>每個源點擴出自己的範圍</figcaption>
</figure>

範圍重疊的地方代表他們同時是多個源點的最短路徑，這個就是我們可以取的答案，我們可以用一個數字 x 來控制每個範圍最多能擴張多少權重，一旦目前的 x 能使某些個範圍重疊的這個 x 就可以是答案，我們二分搜 x 找到最小的 x 使得範圍有重疊

二分搜的複雜度為$ O(\log ⁡C)$ 其中 $C$ 是值域範圍，每次都需要重新擴張源點的範圍(因為擴張的權重上限被更新了)，為 $O(E \log ⁡E )$ 所以複雜度 $O(E \log⁡ E \log⁡ C )$

但我們真的有需要每次都重新算嗎?

這邊提一個結構叫 shortest path tree 又稱最短路徑樹，每個點 v 都跟自己的最短路徑的上一個點 u 連接，形成一顆樹

	我們可以建立最短路徑樹，我們就只需要在樹上 BFS 即可應付每次 x 改變之後的擴張範圍。複雜度 $O(E\log⁡E)$ 建樹，$O(V+E)$ BFS 二分搜 $O(\log ⁡C)$，總共 $O(E \log ⁡E+(V+E)  \log ⁡C)$
	
但其實到頭來我們只是要看重疊的部分，我們也就同樣的建立最短路徑樹，枚舉 edge 使得 (u,v) 是來自不同的源點，ans 去跟他取 min 即可，複雜度 $O(E \log⁡ E+E) = O(E \log⁡ E) $

<figure markdown>
  ![Image title](./images/6.png){ width="300" }
  <figcaption>枚舉重疊邊</figcaption>
</figure>

另解:

對於每個非源點的點都去維護他與最近的兩個「不同的」源點的距離
	
令 f[u] 為 u 的與她最近源點的距離，g[u] 為次近源點的距離，那麼答案就是 $ans =\min⁡(ans,f[u]+g[u])$，至於怎麼建構次短路下面會提到。

### shortest path tree

<figure markdown>
  ![Image title](./images/7.png){ width="300" }
  <figcaption>shortest path tree 結構</figcaption>
</figure>

補: shortest path tree 實作

這邊帶一個相關的題目

???+note "[JOI 2020 p4](https://oj.uz/problem/view/JOI20_ho_t4)"
    给 $n$ 點 $m$ 邊的有向圖
    
    每個邊 ($u_i,v_i,c_i,d_i$) 代表連接 $(u,v),w=c_i$
    
    你最多可以翻轉一邊 翻轉代價是 $d_i$
    
    求從 $1$ 走到 $n$ 再走回 $1$ 的最小 $cost$

    $2 \leq n \leq 200$
    
    $1 \leq m \leq 5 \times 10^4$
    
    $0 \leq c_i \leq 10^6$
    
    $0 \leq d_i \leq 10^9$
    
    ??? note "思路"
    		- 若翻轉的邊在 shortest path tree 上
				- 因為邊被刪掉了，整顆 tree 就要重算
				- 1 → u → v → n
				- 你可以從 1 → u 但這之後要走哪一條? (已經沒有 u → v 了)
			- 若翻轉的邊不在 shortest path tree 上
				- 不經過邊: 原來的答案
				- 要經過邊 1 → v → u → n → 1 或 1 → n → v → u → 1
			- 我們只需要邊在 tree 上時再從新算一次 dijkstra
				- $O(n^3 )$
				- $O(n)$ 樹上最多 n - 1 條邊
				- $O(n^2)$ 暴力 dijkstra


### shortest path DAG

<figure markdown>
  ![Image title](./images/6.png){ width="300" }
  <figcaption>shortest path DAG結構</figcaption>
</figure>

補 : shortest path DAG 實作

???+note "[JOI 2018 p4](https://oj.uz/problem/view/JOI18_commuter_pass)"
	給一張雙向圖和 $s,t,u,v$，從 $s$ 到 $t$ 選一條**最短路徑**，將其邊權都設為 $0$
	
	- 問 $u$ 到 $v$ 的最短路徑最小可以是多少

	??? note "思路"
		- 若有重疊，設重疊為 x→⋯→y
			- ans=dis(u,x)+dis(y,v) or dis(u,y)+dis(x,v)
			- 記得跟 dis(u,v) 取 min
		- 用 shortest path DAG 上枚舉 x 用 dp 得到 y
			- DFS on DAG
			- f[x]= topo sort 在 x 之後的點的 dis(v,y) 最小的 y
		- 同理用 shortest path DAG 找 g[x]=topo sort 在 x 之後的點 dis(u,y) 最小的 y
		- $ans=\min(dis(u,x)+f[x],dis(v,x)+g[x],dis(u,v))$  

補 : 實作




