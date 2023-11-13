???+note "問題"
	有式子 $a^x=b \pmod{p}$，給 $a,b$，找出 $x$
	
設 $x=im+j$ 其中 $m=\sqrt{p}$，且 $1\le i, j\le m$。我們就可以列出 

$a^{im+j}=b\pmod{p}$

$\Rightarrow a^{j}=(a^{-m})^i b\pmod{p}$

預處以所有 $a^j$ 和 $(a^{-m})^i\times b$ 跑過的所有可能，就能找到相同的數，這可以用 map 之類的來實現

???+note "code"
	```cpp linenums="1"
    int BSGS(int a, int b, int p) {
        unordered_map<int, int> R;
        int m = (int)(sqrt(p)) + 1;
        for (int j = 0, t = 1; j <= m; j++) {
            if (R.count(t) == 0)  // 沒出現過
                R[t] = j;         // 加進去
            t = 1ll * t * a % p;
        }
        int a_m = pow(a, m, p);
        int a_m_inv = mod_inv(a_m, p);
        for (int i = 0, t = b; i <= m; i++) {
            if (R.count(t) == 1)  // 找到符合的 j
                return i * m + R[t];
            t = 1ll * t * a_m_inv % p;
        }
        return -1;  // 找不到 x
    }
	```