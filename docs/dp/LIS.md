- LIS, LDS, Dilworth's theorom
- https://zerojudge.tw/ShowProblem?problemid=b903
- sprout oj 421
- TOI 2021 pC
- https://www.luogu.com.cn/training/215

$$dp[i]=\max \limits_{j< i \texttt{ and }a_j<a_i} \{ dp[j] + 1 \}$$

LIS 的圖 : (將 $(i,a_i)$ 打在二維座標上看)