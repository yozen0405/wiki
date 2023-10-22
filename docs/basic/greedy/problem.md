### 2023 TOI 1模 pB
???+note "最佳劇照 (stills)"
    在一維數線上有 $n$ 個 interval $[l_i, r_i]$，要求選擇最少的 point，使得每個 interval 都至少包含一個 point，且所選點的 cost 總和最小。
    
    其中點的 cost 計算方式為：
    
    $$\text{cost}(t) = 有包含 \texttt{ point } t \text{ } 的 \text{ } \texttt{interval} \text{ } 的\text{ } |(r_i - t) - (t - l_i)| \text{ }的總和$$
    
    ??? note "[algoseacow](https://www.facebook.com/algo.seacow/)'s 題解"
        > 步驟一，離散化:
        
        利用線段的開頭 $l_i$ 和結尾 $r_i$ 可以把數線切出 $2n-1$ 個 blocks。
        
        這樣一來，每個 interval 可以當成是某個 block 的左界開始到某個 block 的右界結束。
    
        每個 block 只會有一個最好的 $t$，先計算出一個陣列 $w[0], ..., w[2n-2]$ 表示每個 block 裡面最好的位置的 cost
    
        > 步驟二，計算 $w[i]$：
        
        每個 block 的 $w[i]$ 計算方式 $|(r_i - t) - (t - l_i) |$ 也可以寫成 $2|(l_i+r_i)/2 - t|$ 也就是 $t$ 到 $[l_i, r_i]$ 中間點的距離的兩倍。
        
        也就是說，一個 block 裡面位置 $t$ 的 cost 就是 $t$ 到所有有包含這個 block 的區間的中間點的距離總和的兩倍。
        
        令 $m_i = (l_i+r_i)/2$，
        
        $$|m_1-t| + |m_2-t| + \cdots |m_k-t|$$
        
        這個函數的最小值發生於 $t$ 等於 $m_i$ 的中位數
        
        不過 $t$ 需要被限制在 block 的範圍內，所以需要分幾個 case 討論
    
        - $m_i$ 的中位數在在 block 內，直接讓 $t = m_i$ 的中位數
    
        - $m_i$ 的中位數在在 block 左邊，直接讓 $t =$ block 左界
    
        - $m_i$ 的中位數在在 block 右邊，直接讓 $t =$ block 右界
    
        掃描線的過程需要對每個 block 維護目前有哪些 $m_i$ 可以用，並且維護這些 $m_i$ 的中位數。決定好 $t$ 之後，需要透過一些資料結構在 $O(\log n)$ 時間計算出 $|m_1-t| + |m_2-t| + ... |m_k-t|$，這個部分可以使用線段樹達成。
    
        > 步驟三， dp 算答案：
        
        $dp(i)$ 表示最後一個點選在 block $i$ ，且左界在 block $i$ 以前的所有 interval 都有包含到至少一個點的最好答案。
        
        $dp(i) = \max dp(j) + w[i]$, $j$ 可以選的條件是沒有 interval 開頭結尾都在 block ($j+1$) ~ block ($i - 1$)
        
        這些的可以用來轉移的 $j$ 會是連續的一段，所以可以用套用資料結構快速計算出 $dp(i)$，計算 dp 的總時間是 $O(n) \times$ 查詢時間。資料結構的部分可以用單調隊列 $O(1)$ 查詢，或是用線段樹 $O(\log n)$ 查詢
        > ref : <https://hackmd.io/@algoseacow/rJw_DISe3>

- <https://slides.com/fhvirus/1/fullscreen#/3/7>

???+ note "[TIOJ 1072](https://tioj.ck.tp.edu.tw/problems/1072)"
	有 $n$ 個人要吃飯，第 $i$ 個人想吃的食物需要 $C_i$ 時間才能煮好，而他吃掉食物所花的時間為 $E_i$ ，且廚師同一時間只能煮一個食物，最小化所有人都吃完飯的時間。

	??? note "思路"
		不管哪道食物先煮，總共需要煮的時間都一樣。想要縮短總工時，最好先煮吃飯時間較長的食物。
		<figure markdown>
	    	![Image title](./images/8.png){ width="500"}
	    	  <figcaption><font color="#4472C4">藍色</font>代表煮的時間，<font color="#ED7D31">橘色</font>代表吃的時間</figcaption>
	    </figure>
		??? note "證明"
			假設由演算法得到的吃飯的順序為 $a_1, a_2,\ldots, a_n$ ，則此序列一定滿足特性 $E_{a_i} \ge E_{a_{i+1}}$ 。假設有另外一組吃飯順序為 $b_1, b_2, · · · , b_n$，且不滿足該特性，則一定存在兩個相鄰的人 $b_i , b_i+1$ 滿足 $E_{b_i} < E_{b_{i+1}}$ 。如果將這兩個人的吃飯順序對調， 則考慮第 $j$ 個人吃飯結束的時間 (對調前為 $t_1(j)$ ，對調後為 $t_2(j)$ )，可以以下四種人的情況： 
	
	        1. $j < i$： <br>
	           對調前，結束的時間為 $t_1(j) = \sum \limits_{k=1}^j C_{b_k} + E_{b_j}$；<br>
	           對調後，結束的時間為 $t_2(j) = \sum \limits_{k=1}^j C_{b_k} + E_{b_j}$。  
	        2. $j = i$： <br>
	           對調前，結束的時間為 $t_1(j) = t_1(i)= \sum \limits_{k=1}^{i-1} C_{b_k} + C_{b_i}+E_{b_i}$ <br>
	           對調後，結束的時間為 $t_2(j) = t_2(i)= \sum \limits_{k=1}^{i-1} C_{b_k} + C_{b_{i+1}}+C_{b_i}+E_{b_i}$ 。
	
	        3. $j = i + 1$： <br>
	           對調前，結束的時間為 $t_1(j) = t_1(i+1)= \sum \limits_{k=1}^{i-1} C_{b_k} + C_{b_i}+C_{b_{i+1}}+E_{b_{i+1}}$ ； <br>
	           對調後，結束的時間為 $t_2(j) = t_2(i+1)= \sum \limits_{k=1}^{i-1} C_{b_k} + C_{b_{i+1}}+E_{b_{i+1}}$。 
	
	        4. $j > i + 1$： <br>
	           對調前，結束的時間為 $t_1(j) = \sum \limits_{k=1}^j C_{b_k} + E_{b_j}$； <br>
	           對調後，結束的時間為 $t_2(j) = \sum \limits_{k=1}^j C_{b_k} + E_{b_j}$。  
	
	        我們要比較的是 $\max\{t_1(j)\}$ 和 $\max\{t_2(j)\}$ $(1 \le j \le n)$ ，可以發現會讓 $t_1(j)$ 和 $t_2(j)$ 不同值的只有 $j = i$ 和 $j = i + 1$ ，而且 
	
	        $$\begin{cases}t_1(i + 1) \ge t_2(i) \\ t_1(i + 1) \ge t_2(i + 1)\end{cases}$$
	
	        所以 $\max\{t_1(j)\} \ge \max\{t_2(j)\}$，也就是對調之後，最後吃完的時間一定不會比對調前差。 最後，經過不斷的兩兩對調，一定可以將序列 $b$ 變成序列 $a$ 。最後吃完的時間必為非嚴格遞減，得證序列 $a$ 是這個問題的最優解。
	        
	        > ref : <https://www.csie.ntu.edu.tw/~sprout/algo2021/homework/hand05.pdf>

???+note "[TIOJ 1579.來自未來的新台幣](https://tioj.ck.tp.edu.tw/problems/1579)"
	給長度為 $n$，⾯額分別為 $1,5,10,50,100,500,1000,\ldots$ 的錢幣，第 $i$ 個錢幣的數量為 $c_i$，能湊出的金額共有多少種
	
	$1\le n\le 19$
	
	??? note "思路"
		若小的足以表達大的，就將大的都換成小的。例如 1 元有 5 個，5 元有 2 個，那麼因為 1 * 5 > 5，所以可將 5 都換成 1 元，變成 1 元有 15 個，若 1 元只有 3 個，那麼湊不出來 4，也就不能換了，兩者變成獨立的。
		
		所以我們就依序看大的能不能換成當前最小的即可，最後的答案為 $(c_1 + 1)\cdot (c_2 + 1) \ldots (c_k+1) - 1$
		
		> 參考自 : [PTT](https://www.ptt.cc/bbs/tutor/M.1304705220.A.35E.html)
		
	??? note "code"
		```cpp linenums="1"
		#include <bits/stdc++.h>
	    #define int long long
	    #define pb push_back
	    #define mk make_pair
	    #define F first
	    #define S second
	    #define ALL(x) x.begin(), x.end()
	
	    using namespace std;
	    using pii = pair<int, int>;
	
	    const int INF = 2e18;
	    const int maxn = 3e5 + 5;
	    const int M = 1e9 + 7;
	
	    int a[maxn], cnt[maxn];
	
	    signed main() {
	        int n;
	        cin >> n;
	
	        a[0] = 1;
	        for (int i = 1; i < n; i++) {
	            if (i & 1) {
	                a[i] = a[i - 1] * 5;
	            } else {
	                a[i] = a[i - 1] * 2;
	            }
	        }
	
	        int cur = 0;
	        for (int i = 0; i < n; i++) {
	            cin >> cnt[i];
	            if (i == 0) {
	                continue;
	            }
	            if (a[cur] * cnt[cur] >= a[i]) {
	                cnt[cur] += (a[i] / a[cur]) * cnt[i];
	                cnt[i] = 0;
	            } else {
	                cur = i;
	            }
	        }
	        int ans = 1;
	        for (int i = 0; i < n; i++) {
	            if (cnt[i]) {
	                ans = (ans * ((cnt[i] + 1) % M)) % M;
	            }
	        }
	        cout << (ans - 1 + M) % M << '\n';
	    }  
	    ```

## 交換法

???+note "[APCS 物品堆疊](https://zerojudge.tw/ShowProblem?problemid=c471)"

???+note "[CF 559B](https://codeforces.com/problemset/problem/559/B)"

## 霍夫曼編碼

???+note "問題"
    給定每種字元的出現頻率 $freq_i$，目標是找到一種 Prefix Code 讓 
    
    $$WPL=\sum |s_i| \times freq_i$$
    
    最小化。Prefix Code: 每種字元用一個 0/1 字串 $s_i$ 表達，互相不是 prefix

作法: 每次合併兩個頻率最小的字元

例如說有 4 個點（字元）a、b、c、d，他們的 freq 分別為 7、5、2、4

構樹過程: 

<figure markdown>
  ![Image title](./images/11.png){ width="300" }
</figure>

若需要構造的話，從 root 開始，往左分配 0 的編碼，往右分配 1 的編碼，每個 leaf 的編碼就是從 root 到自己的編碼串起來。
		
???+note "k 叉哈夫曼樹 [洛谷 P2168  [NOI2015]荷马史诗](https://www.luogu.com.cn/problem/P2168)"
	有 $n$ 種字元，第 $i$ 種出現次數為 $w_i$。要用 $k$ 進制的字串 $s_i$ 來代替第 $i$ 種字元，使得:

    - 對於任意的 $1 \le i, j \le n$，$i\neq j$，$s_i$ 都不是 $s_j$ 的前綴

    問重新編碼後的字串最短長度，與最長的 $s_i$ 最短可以是多少

    $n \le 10^5, k \le 9$
    
    ??? note "思路"
		當 $k=2$ 時，就是 Huffman Code 裸題。$k>2$ 的 case，若直接 Greedy 的合併，在最後一次的循環時，Heap 的大小在 $2\ldots k-1$（不足以取出 $k$ 個），那麼整個 Huffman Tree 的 root 的節點個數就會小於 $k$，此時若將一些深度最大的 leaf 拔掉，接到 root 的下方，會使答案變小（若不取深度最大的，則將深度最大的拔起來接到空出來的位置更優）。所以最後的 Huffman Tree 就長成: 所有點的小孩都是滿的，除了最深的一個 internal node 會空出一些位置。具體做法有兩種

        1. 我們可以先將 $2+(n-2)\% (k-1)$ 個節點合併（即形成最深的 internal node），剩下的每次合併 $k$ 個節點即可
        2. 我們補一些額外 $w_i=0$ 的點，這樣這些點就會填滿空出的位置，而且又不影響答案。

        那麼第二個答案其實就直接看樹的高度即可。如果有一個點可以移動到比較小的深度（也就是讓樹的高度變小），那麼字串長度總和也會跟著變小，跟最佳解條件矛盾。
        
    ??? note "code"
    	```cpp linenums="1"
    	// Code by KSkun, 2018/7
        #include <cstdio>
        #include <cctype>
        #include <cstring>

        #include <algorithm>
        #include <vector>
        #include <queue>

        typedef long long LL;

        inline char fgc() {
            static char buf[100000], *p1 = buf, *p2 = buf;
            return p1 == p2 && (p2 = (p1 = buf) + fread(buf, 1, 100000, stdin), p1 == p2)
                ? EOF : *p1++;
        }

        inline LL readint() {
            register LL res = 0, neg = 1; register char c = fgc();
            for(; !isdigit(c); c = fgc()) if(c == '-') neg = -1;
            for(; isdigit(c); c = fgc()) res = (res << 1) + (res << 3) + c - '0';
            return res * neg;
        }

        const int MAXN = 100005;

        int n, k;

        typedef std::pair<LL, LL> PII64;
        std::priority_queue<PII64, std::vector<PII64>, std::greater<PII64> > pq;

        int main() {
            n = readint(); k = readint();
            for(int i = 1; i <= n; i++) {
                LL w = readint();
                pq.push(PII64(w, 0));
            }
            while(k > 2 && n % (k - 1) != 1) {
                pq.push(PII64(0, 0)); n++;
            }
            LL ans1 = 0, ans2 = 0;
            while(pq.size() > 1) {
                PII64 res(0, 0);
                for(int i = 1; i <= k; i++) {
                    res.first += pq.top().first;
                    res.second = std::max(res.second, pq.top().second + 1);
                    pq.pop();
                }
                ans1 += res.first;
                ans2 = std::max(ans2, res.second);
                pq.push(res);
            }
            printf("%lld\n%lld", ans1, ans2);
            return 0;
        }
    	```
		

???+note "[CF 1882 C. Card Game](https://codeforces.com/contest/1882/problem/C)"
	給一個長度為 $n$ 的陣列 $a_1, \ldots ,a_n$，每次操作可以 :
	
	- 移除一個奇數項，得分加上 $a_i$
	
	- 移除一個偶數項
	
	每次操作完後陣列都會 reindexed。可以做任意次操作，問最大得分
	
	$n\le 2\times 10^5, -10^9 \le a_i \le 10^9$
	
	??? note "思路"
		令 a 為要取的 odd 項，b 為要取的 even 項。例如陣列是 a..b.bab...b..a..b，我們可以先取最後的 a，這樣後面的 b 就會變 odd，再由後往前取，然後重複這個步驟，就可以取完。但可能會有一個 case 例如 .b.b.bab...b，這樣最前面的三個 b 無論如何都是沒辦法取的，必須捨棄，我們必須在前面再取一個才能讓這三個 b 取到。
		
		所以我們得到了一個 greedy 的結論，將 $i$-th 移除，我們可以得到 $\sum\limits_{j>i} \max (0, a_j)$，所以枚舉第一個選的，然後將 ans 與他的答案取 max
		
	??? note "code"
		```cpp linenums="1"
		scanf("%d",&n);
		for(int i = 1; i <= n; i++) {
			scanf("%d",&a[i]);
		}
		long long s=0;
		for (int i = n; i >= 1; i--) {
			if(i&1) {
				ans = max(ans, s + a[i]);
			} else {
				ans = max(ans, s);
			}
			s += max(a[i], 0);
		}
		printf("%lld\n", ans);
		```

???+note "[CSES - Programmers and Artists](https://cses.fi/problemset/task/2426)"
	給你 $n$ 個 pair$(x_i,y_i)$，要你選這些 pair 裡面的 $a$ 個 $x$ 跟 $b$ 個 $y$，且同一個 pair 中的 $x$ 和 $y$ 不能同時挑
	
	$n\le 2\times 10^5$
	
	??? note "思路"	
		<https://github.com/yozen0405/c-projects/blob/main/markdown/2426.md>

???+note "[JOI Final 2022 選舉](https://loj.ac/p/3664)"
	有 $n$ 個州，若在第 $i$ 個州演講 $a_i$ 小時可獲得一張選票，若演講 $b_i$ 小時可獲得一位協作者。多一個協作者就可讓時間加速兩倍，問要獲得 $k$ 張選票的最小耗時
	
	$k\le n\le 500, 1\le a_i\le 1000, a_i\le b_i\le 1000$ 或 $b_i=-1$
	
	??? note "思路"
		容易想到一個錯誤的貪心: 
		
		- 按照 $b_i$ 小到大排序
		
		- 枚舉分界線
			- 前面都選 $b_i$
	
			- 後面選最小的幾個 $a_i$
		
		但會發現，可能一些選中的 $b_i$ 對應的 $a_i$ 很小，這時，我們挑選這些 $a_i$，可能會更好。舉例來說:
		
		```
		a   b
		------
		1   99
		99  100
		100 101
		```
		
		這時若我們以 b, b, a 的方式挑，耗時 $99+100/2+100/4=174$ 小時，但若我們以 a, b, a 的方式挑耗時 $1/2+100/2+100=150.5$，更優。
		
		所以就變成: 
		
		- 按 $b_i$ 小到大排序
	
		- 枚舉分界線: 
			- 分界線前，對於每一個要馬選 $a_i$，要馬選 $b_i$ → dp
	
			- 對於後面，選 $a_i$ 最小的 $k-i$ 個 → 預處理