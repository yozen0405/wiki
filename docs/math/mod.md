## Modular Arithmetic

- (a + b) % m ≡ (a % m + b % m) % m
- (a * b) % m ≡ (a % m * b % m) % m
- (a - b) % m ≡ (a % m - b % m) % m

??? warning "負數 mod 要寫成 ((a - b) % m + m) % m"
	通常會把 (a - b) % m 寫成 ((a - b) % m + m) % m 來保證在進行 mod m 運算時是正數

## 模逆元

$a\times b \equiv 1 \pmod{m}$，其中 $a$ 和 $m$ 互質。

$b  \equiv a^{-1} \pmod{m}$，稱 $b$ 是 $a$ 在模 $m$ 下的模逆元

??? question "為什麼除以一個數取模等於乘以這個數的模逆元"
	令 $c$ 為 $b$ 在 $\text{mod } p$ 之下的模逆元

	$\text{proof : }(a/b) \pmod{p} \equiv a \times c \pmod{p}$
	
	$(b\times c)\equiv 1$
	
	$b\times c\equiv 1 \pmod{p}$
	
	$a/b=(a/b)\times 1 \equiv (a/b)\times (b\times c) \pmod{p}=a\times c\pmod{p}$

找模逆元以下會分成 m 是質數與 m 不是質數有對應的做法

### 小費馬定理

???+info "小費馬定理"
	$a^{p - 1} = 1 \pmod{p}$，其中 $p$ 為質數且 $a$ 與 $p$ 互質

【證明】

因為 $\gcd(a,p)=1$，考慮 $1\times a, 2\times a, 3\times a, \ldots, (p - 1)\times a$，在 $\text{mod } p$ 的集合恰為 $\{1,2,3,\ldots ,(p-1) \}$ 的重新排列[^1]，因此我們可以列出

$$\begin{align*} & (1\times a)\times (2\times a) \times \cdots \times ((p-1)\times a) \equiv 1\times 2\times  \cdots \times (p-1)   \pmod{p} \\ & \Rightarrow a^{p-1} \times 1 \times 2\times \cdots \times (p-1)\equiv 1\times 2\times \cdots \times (p-1)  \pmod{p} \\ & \Rightarrow (a^{p-1} - 1)\times (1\times 2\times \cdots \times (p-1))\equiv 0 \pmod{p}
\end{align*}$$

因為 $p$ 與 $1\times 2\times \cdots \times (p-1)$ 互質，所以

$$a^{p-1}-1\equiv 0 \pmod{p} \Rightarrow a^{p-1}\equiv 1\pmod{p}$$

### 小費馬定理求模逆元

$$a^{p-2}\equiv a\times a^{p-2}\equiv 1 \pmod{p}$$

故 $a$ 在模 $p$ 下的模逆元為 $a^{p-2}$，可用快速冪在 $O(\log n)$ 下求解

【注意】 : 小費馬定理求模逆元只適用於 m 是質數

??? note "code"
	```cpp linenums="1"
	int fastpow(int a, int b, int M) {
        int ret = 1;
        while (b != 0) {
            if (b & 1) ret = (ret * a) % M;
            a = (a * a) % M;

            b >>= 1;
        }
        return ret;
    }
    
    int get_inv(int a, int m) {
    	return fastpow(a, m - 2, m);
    }
    ```

### Extended GCD 

??? info "輾轉相除法 : $\gcd(a,b)=\gcd(b,b \% a)$"
	令 $d=\gcd(a,b),a=n\times d,b=m\times d$，一直互相相減之後就會得到 $d$
	
	> 詳見 : <https://hackmd.io/@Koios/rJ_lER719>

???+info "貝祖定理"
	在 $ax+by=m$ 中， 若且唯若 $m$ 是 $a$ 及 $b$ 的最大公因數 $\gcd(a,b)$ 的倍數，有整數解

【證明】

根據貝祖定理，我們列出 

$$ax+by =\gcd(a,b)$$

套用輾轉相除法

$$\begin{align}& bx_1+(a \% b)y_1=\gcd(a,b) \\ \Rightarrow\space  & bx_1+(a-b\times \lfloor{\frac{a}{b}} \rfloor) y_1=\gcd(a,b)\end{align}$$

兩式皆為 $\gcd(a,b)$，列出 :

$$ax+by=bx_1+(a-b\times \lfloor{\frac{a}{b}} \rfloor) y_1$$

得

$$x=y_1,y=x_1 - \lfloor{\frac{a}{b}} \rfloor y_1$$

??? note "code"
	```cpp linenums="1"
	pii extgcd(int a,int b) {
        if (b == 0) {
        	// a * x + 0 * y = gcd(a, 0) = a
            return {1, 0};
        }
        pii p = extgcd(b, a % b);
        return {p.S, p.F - (a / b) * p.S};
    }
    ```
    
### 貝祖定理求模逆元

貝祖定理: $ax + by = \gcd(a, b)$

$b = m$ 代入得 $ax + my = \gcd(a, m) = 1$

兩邊同 $\text{mod}\space  m$ 得  $ax = 1 \pmod{m}$

$x = a^{-1}$ 為 $a$ 在模 $m$ 下的模逆元


【注意】 : 在 m 不是質數的時候也可以用貝祖定理求模逆元

??? note "code"
	```cpp linenums="1"
	pii extgcd(int a,int b) {
        if (b == 0) {
        	// a * x + 0 * y = gcd(a, 0) = a
            return {1, 0};
        }
        pii p = extgcd(b, a % b);
        return {p.S, p.F - (a / b) * p.S};
    }
    
    int get_inv(int a, int m) {
    	pii p = extgcd(a, m);
    	int x = p.F;
    	return (x % m + m) % m;
    }
    ```

[^1]: 見<a href="/wiki/math/images/7.png" target="_blank">此處</a>
