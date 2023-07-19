???+note "[USACO 2021 December Contest, Silver Problem 3. Convoluted Intervals](https://www.usaco.org/index.php?page=viewproblem2&cpid=1160)"
	給 $n$ 個 intervals $(a_i,b_i)$。問對於 $k=0\sim 2m$ 的每個 $k$ 問有多少 pair $(i,j)$ 滿足 $a_i + a_j \leq k \leq b_i + b_j$
	
	$n\le 2\times 10^5,m\le 5000,0\le a_i,b_i \le m$
	
	??? note "思路"
		因為 $m$ 只有 $5000$，我們將開一個大小為 $m$ 的 vector，第 $x$ 格存 $a_i=x$ 的 $i$ 的數量，直接 $m^2$ 枚舉 vector 裡面的 index
		
		> 參考自 : <https://www.usaco.org/current/data/sol_prob3_silver_dec21.html>
		
	??? note "code(from usaco)"
		```cpp linenums="1"
		#include <bits/stdc++.h>
        using namespace std;

        int main() {
            cin.tie(0)->sync_with_stdio(0);
            int N, M;
            cin >> N >> M;
            vector<pair<int, int>> ivals(N);
            for (auto &ival : ivals)
                cin >> ival.first >> ival.second;
            vector<int64_t> win_start(2 * M + 1), win_end(2 * M + 1);
            {
                vector<int64_t> a_freq(M + 1);
                for (int i = 0; i < N; ++i)
                    ++a_freq.at(ivals.at(i).first);
                for (int i = 0; i <= M; ++i)
                    for (int j = 0; j <= M; ++j)
                        win_start.at(i + j) += a_freq.at(i) * a_freq.at(j);
            }
            {
                vector<int64_t> b_freq(M + 1);
                for (int i = 0; i < N; ++i)
                    ++b_freq.at(ivals.at(i).second);
                for (int i = 0; i <= M; ++i)
                    for (int j = 0; j <= M; ++j)
                        win_end.at(i + j) += b_freq.at(i) * b_freq.at(j);
            }
            int64_t win_count = 0;
            for (int i = 0; i <= 2 * M; ++i) {
                win_count += win_start.at(i);
                cout << win_count << "\n";
                win_count -= win_end.at(i);
            }
        }
		```