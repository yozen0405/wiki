## Majority Voting

???+note "[LeetCode 169. Majority Element](https://leetcode.com/problems/majority-element/)"
	給一個長度為 $n$ 的陣列，輸出出現超過 $\lfloor n/2\rfloor$ 次的數字
	
	$n\le 5\times 10^4$
	
	??? note "code"
        ```cpp linenums="1"
        class Solution {
        public:
            int majorityElement(vector<int>& nums) {
                int v = -1, c = 0;
                for(int x : nums) {
                    if (v == -1 && c == 0) {
                        v = x;
                        c = 1;
                        continue;
                    }

                    if (v != x) {
                        c--;
                        if (c == 0) v = -1;
                    } else {
                        c++;
                    }
                }

                c = 0;
                for(int x : nums) {
                    if (x == v) c++;
                }

                int n = nums.size();
                if (c > (n / 2)) return v;
                else return -1;
            }
        };
        ```

$x,y$ 打架，分兩種 case :

- $x,y$ 不同 $\Rightarrow x,y$ 各減少一個

- otherwise $\Rightarrow$ 都保留

??? question "可否再線段樹上實現 ?"
    參考<a href="/wiki/ds/segment_tree/#_2" target="_blank">此處</a>
    
## 1/k

???+note "[LeetCode 229. Majority Element II](https://leetcode.com/problems/majority-element-ii/)"
	給一個長度為 $n$ 的陣列，輸出所有出現超過 $\lfloor n/3\rfloor$ 次的數字
	
	$n\le 5\times 10^4$
	
	??? note "思路"
		$x,y,z$ 打架，分兩種 case :

        - $x,y,z$ 都不同 $\Rightarrow x,y,z$ 各減少一個

        - otherwise $\Rightarrow$ 都保留
	
	??? note "另解"
		nth_element 找 rank n/3, 2n/3, 3n/3，然後檢查這些位置的數字出現次數是否 >= [n / 3]
		
		因為如果一個數字出現超過 n/3 的話那一定至少碰到 n/3, 2n/3, 3n/3 其中一個
		
		```
		------O------O------O
		```
		
	??? note "code"
        ```cpp linenums="1"
        class Solution {
        public:
        const int INF = 1e9 + 7;
            vector<int> majorityElement(vector<int>& nums) {
                int v1 = -INF, c1 = 0;
                int v2 = -INF, c2 = 0;

                for (int x : nums) {
                    if (c1 > 0 && x == v1) {
                        c1++;
                        continue;
                    }
                    if (c2 > 0 && x == v2) {
                        c2++;
                        continue;
                    }

                    if (c1 == 0) {
                        v1 = x;
                        c1 = 1;
                    } else if (c2 == 0) {
                        v2 = x;
                        c2 = 1;
                    } else {
                        c1--;
                        if (c1==0) v1 = -INF;
                        c2--;
                        if (c2==0) v2 = -INF;
                    }
                }
                c1 = 0, c2 = 0;
                for (int x : nums) {
                    if (x == v1) c1++;
                    if (x == v2) c2++;
                }
                vector<int> ans;
                int n = nums.size();
                if (c1 > n/3) ans.push_back(v1);
                if (c2 > n/3) ans.push_back(v2);
                return ans;
            }
        };
        ```

??? info "正確性證明"
	如果有一個東西出現超過 $\lfloor n/3\rfloor$，那他不可能全部都被丟掉，因為當且僅當 $x,y,z$ 只有在都不一樣的時候才會被丟掉
	
---

## 參考資料

- <https://abc864197532.github.io/2021/02/07/tioj-2140/>

-  <https://codeforces.com/blog/entry/89880>

- <https://zh.wikipedia.org/zh-tw/多数投票算法>