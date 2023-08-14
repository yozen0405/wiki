## 介紹

### 將 query 排序

分成 sqrt(n) 個 block

每筆 Query 按照 : 

1. 左界所屬 Block 從小排至大

2. 若左界 block 相同，則將右界從小排至大

???+note "code"
	```cpp linenums="1"
	struct Query {
        int l, r, block_id;

        bool operator<(const Query &rhs) const {
            if (block_id != rhs.block_id) {
                return block_id < rhs.block_id;
            } else {
                return r < rhs.r;
            }
        }
    };
    ```
	
### 暴力移動 pointer

注意先擴大（add），再縮小（del）

???+note "code"
	```cpp linenums="1"
	for (auto &i : Query) {
		while (l > i.l) add(--l);
		while (r > i.r) add(++R);
		while (L < i.l) del(l++);
		while (r > i.r) del(r--);
	}
	```

### 維護 data structure

- O(1) 新增/刪除

- O(sqrt(n)) 查詢

    - 總和
    - x 出現的次數
    - mode
    - 第 k 小

### 複雜度

- left pointer

    - 同塊 : 同個 block 裡面 l 最多移動 sqrt(n) 格，O(Q * sqrt(n))
    - 不同 : 不同 block 之間的總移動距離為 O(N)

- right pointer

    - 同塊 : 每個 block 裡面 r 最多會移動 N 格，共 sqrt(N) 個 block，O(N * sqrt(N))
    - 不同 : 換 block 最多 sqrt(N) 次，每次最多移動 N 格，O(N * sqrt(N))

每 k 個當一個 block :

- left: O(Q * k)

- right: O(N * (N / k))

???+note "[CF 86 D. Powerful Array](https://codeforces.com/problemset/problem/86/D)"

	給一個長度為 $n$ 的陣列 $a_1,\ldots ,a_n$，有 $q$ 筆詢問 :
	
	- 給一個區間 $[l,r]$，將區間內每種數字與其出現的次數平方的乘積加總後輸出
	
	$n,q\le 2\times 10^5,1\le a_i \le 10^6$
	
	??? note "思路"



## 值域



---

## 參考資料

- <https://sam571128.codes/2020/10/03/MO-Algorithm/>

- <https://cp.wiwiho.me/mo-algorithm/>

- <https://docs.google.com/document/d/11-Ho9_nnds76VdfBfDQC7U1sYjXVrlNgEurdZ4ykL4s/edit#heading=h.isbo4ewxeenw>