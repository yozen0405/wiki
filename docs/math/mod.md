## Modular Arithmetic

- (a + b) mod m ≡ ((a mod m) + (b mod m)) mod m
- (a * b) mod m ≡ ((a mod m) * (b mod m)) mod m
- (a - b) mod m ≡ ((a mod m) - (b mod m)) mod m

??? warning "負數 mod 要寫成 : ((a - b) mod m + m) mod m"
	通常會把 (a - b) mod m 寫成 ((a - b) mod m + m) mod m來保證在進行 mod m 運算時是正數
	
## 輾轉相除法

gcd(a, b) = gcd(b, a%b)

## Extended GCD 

???+info "貝祖定理"
	在 $ax+by=m$ 中， 若且唯若 $m$ 是 $a$ 及 $b$ 的最大公因數 $\gcd(a,b)$ 的倍數，有整數解

$$\begin{align*}
    ax+by & =\gcd(a,b) \\
    & = \gcd(b, a \bmod b) \\
    & = \gcd(b, a - \lfloor \frac{a}{b} \rfloor b) \\
    & = bx' + (a - \lfloor \frac{a}{b} \rfloor b)y' \\
    & = ay' + b(x' - \lfloor \frac{a}{b} \rfloor y)
\end{align*}$$

??? note "code"
	```cpp linenums="1"
	pii ex_gcd(int a,int b) {
        if (b == 0) {
            return {1, 0};
        }
        pii p = ex_gcd(b, a%b);
        int x = p.second;
        int y = p.first - p.second*(a/b);
        return {x,y};
    }
    ```