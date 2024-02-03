- JOI 2016 p3
- 2015 nhspc p5
- 2022 toi mock 1 pA
- 2021 nhspc pD

拓樸排序:DAG 上某一種節點的順序

$v_1, v_2, . . . , v_n$ ,使得對於所有 i > j,圖上不存在從 i 到 j 的路徑

換句話說,就是如果可以從一個節點 u 走到另一個節點 v,那麼在拓樸排序上,u 要出現在 v 前面

可能不唯一

## 判環

如果 queue 空了以後,還有節點沒拔掉,就代表有環

## 同一個 level 的放一起

???+note "code"
	```cpp linenums="1"
	void topo() {
        queue<int> q;
        while (q.size()) {
            queue<int> nq;
            while(q.size()) {
                auto u = q.front(); 
                q.pop();

                for (auto v : G[u]) {
                    deg[v]--;
                    if (deg[v] == 0) {
                        nq.push(v);
                    }
                }
            }
            q = nq;
        }
    }
    ```
    
## 例題
    
???+note "[2023 全國賽 C. 與自動輔助駕駛暢遊世界 (Autocopilot)](https://sorahisa-rank.github.io/nhspc-fin/2023/problems.pdf#page=11)"
	給一張 n 點 m 邊的有向圖，有一個機器人要從起點 s 跟終點 t。當遇到叉路，他會隨機走其中一條，而我們可以花費一個硬幣，可以強制決定他要走的方向，問至少要帶多少錢，才可以保證不會走到死路
	
	$n\le 3000, m\le 3\times 10^4$
	
	??? note "思路"
		【Subtask: 圖是 DAG】
		
		先判斷: 
		
		- 哪些點是一定走不到 t，dead node

		- 哪些點是有機率走到 t

		- 哪些點是一定走得到 t

		這可以利用從 out degree = 0 的 node 去 dfs 出去來判斷。之後，我們就可以利用 DAG dp 來看每個點最少要付多少錢。令 dp(u) 從 u 走到終點 t，最少要付多少錢，我們可以列出轉移式:
		
		- 如果 u 不可能走到 t, dp(u) = INF
		
		- dp(u) = $\min_{u→v} \{$ 
			- 要花錢，$1 + \min\{ dp(v) \}$
			
			- 不花錢 $\max\{ dp(v) \}$

???+note "[USACO 2013 JAN Party Invitations S](https://www.luogu.com.cn/problem/P3068)"
    有 n 頭牛，每頭牛有它自己的朋友圈，沒有一個完全與之相同的。假設該牛朋友圈有 k 頭牛，若已經邀請了 k - 1 頭，那麼剩下的那頭牛也得邀請。問再邀請 1 號牛的情況下，最少需要邀請多少頭乳牛?

	$n\leq 10^6, \sum k \leq 2.5 \times 10^5$
	
	??? note "思路"
		我們可以把每頭牛想成是一個狀態，利用拓樸排序解決問題。我們利用 vector 存與 i 有關的朋友圈編號，用 set 存朋友圈的集合。
		
		第一次就是將 1 加入 queue，然後循環 vector，把循環到的集合中的 1 都刪去，判斷刪去後的集合大小是否為 1，如果大小是 1，就入隊，重複操作。坑點就是，出來的可能會被重複做，加一個陣列判斷一下是否已經選了這頭奶牛。
		
	??? note "code"
		```cpp linenums="1"
		#include <cstdio>
        #include <iostream>
        #include <queue>
        #include <set>
        #include <vector>
        using namespace std;

        int n, m, ans, vis[1000005];
        set<int> s[250005];
        vector<int> G[1000005];
        queue<int> q;

        int main() {
            cin >> n >> m;
            for (int i = 1; i <= m; i++) {
                int t;
                cin >> t;
                for (int j = 1; j <= t; j++) {
                    int x;
                    cin >> x;
                    G[x].push_back(i);
                    s[i].insert(x);
                }
            }
            q.push(1);
            vis[1] = 1;
            while (!q.empty()) {
                int now = q.front();
                q.pop();
                ans++;
                for (int i = 0; i < G[now].size(); i++) {
                    s[G[now][i]].erase(now);
                    if (s[G[now][i]].size() == 1 && !vis[*s[G[now][i]].begin()]) {
                        int t = *s[G[now][i]].begin();
                        q.push(t);
                        vis[t] = 1;
                    }
                }
            }
            cout << ans << '\n';
            return 0;
        }
		```
