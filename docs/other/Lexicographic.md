- TIOJ 2052
- Leetcode 440.K-th Smallest in *Lexicographical Order*

string $a$ 比 string $b$ 字典序還小符合以下條件之一

- $a$ 是 $b$ 的 prefix

- 存在 $1\le i\le \min(|a|, |b|)$ 使得 $a_i < b_i$，且 $1\le j < i$，$a_j=b_j$

利用 c++ string operator `<` 可以判斷兩個字串的字典序，複雜度 O(n)