## 介紹

- 持久化並查集

- 帶權並查集 (CF imposter)

- 可撤銷並查集 (2023 成大賽 p4)

- 加點技巧 (海牛 class 11 hm)

## 用途

???+note "[洛谷 P2024 [NOI2001] 食物链](https://www.luogu.com.cn/problem/P2024)"
	有三類動物 $A,B,C$，這三類動物的⾷物鏈構成如下：$A$ 吃 $B$，
	$B$ 吃 $C$，$C$ 吃 $A$。
	
	現有 $N$ 個動物，編號 $1,2,...,N$。每個動物都是 $A,B,C$ 中的⼀種，但並不知道是哪⼀種。依序給 $K$ 條屬
	於以下兩種的敘述：
	
	- $1\space X\space Y$，表⽰ $X$ 和 $Y$ 是同類
	
	- $2\space X\space Y$，表⽰ $X$ 吃 $Y$
	
	然⽽，並不是每條描述都是正確的，有些是真話，有些是假話。
	如果當前的話與前⾯的某些真話衝突，就是假話請判斷哪些話是真話，哪些話是假話。
	
	??? note "思路"
		- 我們可以⽤並查集維護資訊間的因果關係
	
	    - ⼀個集合裡⾯的資訊代表「這些資訊必須同時發⽣」
	
	    - ⽐如說，假設 $(x,y)$ 符合第⼀種資訊
	
	    - 那就代表：如果 $x$ 是 $A$ 類，那 $y$ 必須是 $A$ 類，反之亦然
	    （$B,C$ 類同樣可以套⽤）
	    
	    - 翻譯⼀下：如果 $x_A$ 發⽣的話，那 $y_A$ 也⼀定要發⽣（$B,C$
	    類同樣可以套⽤）
	    
	    - $\text{join}(x_A, y_A),\text{join}(x_B, y_B), \text{join}(x_C,y_C)$
	
		---
		
		- ⽐如說，假設 $(x, y)$ 符合第⼆種資訊
	
	    - 那就代表：
	    	- 如果 $x$ 是 $A$ 類，那 $y$ 必須是 $B$ 類，反之亦然
	    	- 如果 $x$ 是 $B$ 類，那 $y$ 必須是 $C$ 類，反之亦然
	    - 如果 $x$ 是 $C$ 類，那 $y$ 必須是 $A$ 類，反之亦然
	
	    - $\text{merge}(x_A, y_B),\text{merge}(x_B, y_C), \text{merge}(x_C,y_A)$ 
		
		---
		
		- 矛盾的情況呢 ?
		- 假如 $(x, y)$ 符合第⼆種資訊
		- 如果 $\text{find}(x_A)=\text{find}(y_C)$ 或 $\text{find}(x_A)=\text{find}(y_A)$ 則矛盾
		- 翻譯 : 
			- 如果 $x$ 是 $A$ 類，那 $y$ 必須是 $C$ 類 
			- 如果 $x$ 是 $A$ 類，那 $y$ 必須是 $A$ 類 
		
		---
		
		<figure markdown>
          ![Image title](./images/4.png){ width="200" }
        </figure>
		
		假如今天 $1$ 吃 $2$，$2$ 吃 $3$ 關西如下圖
		
		<figure markdown>
          ![Image title](./images/3.png){ width="300" }
          <figcaption>同一 row 的必同時發生</figcaption>
        </figure>
		
		
		
		---
		
		```cpp linenums="1"
		if (flag == 1) {
			if (check (a, b + n) || check (a, b + 2*n)) {
				ans++;
			}
			else {
				merge (a, b), merge (a + n, b + n), merge (a + 2*n, b + 2*n);
			}
		}
		else {
			if (check (a, b) || check (a, b + 2*n)) {
				ans++;
			}
			else {
				merge (a, b + n), merge (a + n, b + 2*n), merge (a + 2*n, b);
			}
		}
		```
		
		
		 
		

### Uva 11987

???+note "[zerojudge f292. 11987 - Almost Union-Find](https://zerojudge.tw/ShowProblem?problemid=f292)"
	有個 $n$ 物品，每個物品一開始都是自己一組
	
	有 $q$ 次操作，每次會是其中一種
	
	- $\text{Merge}(x,y):$ 將 $x,y$ 所在的兩個群體合併為同一個

	- $\text{MoveGroup}(x,y):$ 將 $x$ 從他所在的群體當中移除並且加入 $y$ 所在的群體

	- $\text{Sum}(x):$ 印出 $x$ 所在的群體包含的成員個數和成員編號總合

	??? note "思路"
		我們先來思考「將 $x$ 從他所在的群體當中移除」
		
		我們直接將 $x$ 的貢獻給扣掉，也不必真正在 DSU 裡將其刪除
		
		接下來思考 「並且加入 $y$ 所在的群體」
		
		我們可以對於 $i=1\sim n$ 維護 $t_i$ 代表目前 $i$ 真正的編號
		
		加入新的 group 的時候只需將 $t_i$ 變成當前沒用過的編號即可，並且可以當成是一個新的點，去執行 merge
		
		詳見代碼
		
		??? note "code"
			```cpp linenums="1"
            void init () {
                for (int i = 1; i <= n; i++) {
                    f[i] = i;
                    t[i] = i;
                    sum[i] = i;
                    num[i] = 1;
                }
                cnt = n;
            }

            void delete (int x) {
                sum[find (t[x])] -= x;
                num[find (t[x])] -= 1;

                t[x] = ++cnt;
                sum[t[x]] = x;
                num[t[x]] = 1;
                f[t[x]] = t[x];
            }

            void merge (int x, int y) {
                int tx = find (t[x]);
                int ty = find (t[y]);
                if (tx != ty) f[ty] = tx;
                num[tx] += num[ty];
                sum[tx] += sum[ty];
            }

            void solve (int x, int y) {
                delete (x);
                merge (x, y);
            }
            ```
			> 參考自 : [CSDN](https://blog.csdn.net/weixin_52914088/article/details/120379127?ops_request_misc=&request_id=&biz_id=102&spm=1018.2226.3001.4187)
			
- DSU 判環
- https://blog.csdn.net/boliu147258/article/details/92778897

---

## 參考資料

- <https://zhuanlan.zhihu.com/p/553192435>
- [Codeforces Edu DSU (需加入 group)](https://codeforces.com/edu/course/2/lesson/7)