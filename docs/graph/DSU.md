## 介紹

- 持久化並查集

- 帶權並查集 (CF imposter)

- 可撤銷並查集 (2023 成大賽 p4)

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
		- 為什麼不用考慮 $\text{find}(x_B)=\text{find}(y_A)$ 的情況呢 ?
			- 因為若 $(x,y)$ 之前有 $\text{merge}$ 過那他們一定三個分別在三個集合
			- 就只要看當時 $\text{merge}$ 的時候是符合那一種 case
			- 他是屬於一種環形結構 $A\rightarrow B \rightarrow C \rightarrow A$

- DSU 判環
- https://blog.csdn.net/boliu147258/article/details/92778897

---

## 參考資料

- <https://zhuanlan.zhihu.com/p/553192435>
- [Codeforces Edu DSU (需加入 group)](https://codeforces.com/edu/course/2/lesson/7)