## 雙向 BFS

???+note "[Leetcode 752. Open the Lock](https://leetcode.com/problems/open-the-lock/)"
	有一個滾輪式四位密碼圈，初始的密碼為 0000，每次可以轉其中一位一格。給定一些不合法的密碼，和目標密碼，問最少轉幾次可達到目標密碼，且途中不出現不合法密碼
	
	不合法密碼數量 $\le 500$
	
	??? note "code"
		```cpp linenums="1"
		class Solution {
           public:
            string s, t;
            unordered_set<string> st;
            int openLock(vector<string>& deadends, string target) {
                s = "0000";
                t = target;
                if (s == t) return 0;
                for (const auto& d : deadends) st.insert(d);
                if (st.count(s)) return -1;
                int ans = bfs();
                return ans;
            }
            int bfs() {
                queue<string> d1, d2;
                unordered_map<string, int> m1, m2;
                d1.push(s);
                m1[s] = 0;
                d2.push(t);
                m2[t] = 0;
                while (d1.size() and d2.size()) {
                    int t = -1;
                    if (d1.size() <= d2.size()) {
                        t = update(d1, m1, m2);
                    } else {
                        t = update(d2, m2, m1);
                    }
                    if (t != -1) return t;
                }
                return -1;
            }
            int update(queue<string>& q, unordered_map<string, int>& cur, unordered_map<string, int>& other) {
                int m = q.size();
                while (m-- > 0) {
                    string t = q.front();
                    q.pop();
                    int step = cur[t];
                    for (int i = 0; i < 4; i++) {
                        for (int j = -1; j <= 1; j++) {
                            if (j == 0) continue;
                            int origin = t[i] - '0';
                            int next = (origin + j) % 10;
                            if (next == -1) next = 9;
                            string copy = t;
                            copy[i] = '0' + next;
                            if (st.count(copy) or cur.count(copy)) continue;
                            if (other.count(copy))
                                return step + 1 + other[copy];
                            else {
                                q.push(copy);
                                cur[copy] = step + 1;
                            }
                        }
                    }
                }
                return -1;
            }
        };
            ```
	
???+note "八位數碼問題 [CSES - Swap Game](https://cses.fi/problemset/task/1670)"
	給一個 3 * 3 的 grid，裡面的數自恰為 1, 2, ..., 9。初始的 grid 如下:
	
	```
	1 2 3
	4 5 6
	7 8 9
	```
	
	問每次 swap 相鄰的兩項最少幾次可變成給定的 grid
	
	??? note "思路"
		