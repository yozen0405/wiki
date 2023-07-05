## Master theorem

$$T(n) = aT(\frac{n}{b}) + f(n)$$

let $\displaystyle c = \frac{\log a}{\log b}  = \log_b a$

case 1: $f(n) = O(n^{c-\epsilon}) \Rightarrow T(n) = \Theta(n^c)$

- $\displaystyle T(n) = 2 T(\frac{n}{2}) + O(n^{0.5}) = O(n), c = 1$
- $\displaystyle T(n) = 3 T(\frac{n}{2}) + O(n) = O(n^c), c = \log_2 3$

case 2: $f(n) = O(n^c \log^k n)$ && $k\ge 0 \Rightarrow T(n) = \Theta(n^c \log^{k+1} n)$

- $\displaystyle T(n) = 2 T(\frac{n}{2}) + O(n) = O(n \log n)$
- $\displaystyle T(n) = T(\frac{n}{2}) + O(1) = O(\log n)$

case 3: $n^{c+\epsilon} = O(f(n)) \Rightarrow T(n) = \Theta( f(n) )$

- $\displaystyle T(n) = 2 T(\frac{n}{2}) + O(n^2) = O(n^2)$

## recursive function 

- $\displaystyle T(n) = 2T(\frac{n}{2}) + O(n) = O(n \log n)$
    - merge sort
        - common divide and conquer algorithms

- $\displaystyle T(n) = T(\frac{n}{2}) + O(1) = O(\log n)$
	- binary search

- $\displaystyle T(n) = T(\frac{n}{2}) + O(n) = O(n)$
	- STL nth_element

- $\displaystyle T(n) = 2T(\frac{n}{2}) + O(\frac{n}{\log n}) = O(n \log^2 n)$

	- some BBST
    - suffix array using std::sort

- $\displaystyle T(n) = 2T(\frac{n}{2}) + O(\frac{n}{\log n}) = O(n \log\log n)$

	- harmonic series (Riemann zeta function when s = 1)

		- $\displaystyle \frac{1}{1} + \frac{1}{2} + \frac{1}{3} + \frac{1}{4} + \ldots + \frac{1}{n} = O(\log n)$

- $\displaystyle T(n) = 2T(\frac{n}{2}) + O(n \log^2 n) = O(n)$
	- Basel problem (Riemann zeta function when s = 2)
		- $\displaystyle (\frac{1}{1})^2 + (\frac{1}{2})^2 + \ldots + (\frac{1}{n})^2+\ldots = \pi^2 / 6 = O(1)$