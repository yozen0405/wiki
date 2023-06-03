```cpp linenums="1"
struct Fraction {
    int b, a; // b/a

    bool operator<(const Fraction &rhs) const {
        return b * rhs < a * rhs;
    }
    Fraction operator+(const Fraction &rhs) const {
        Fraction ret;
        ret.b = b * rhs.a + rhs.b * a;
        ret.a = a * rhs.a;
        int d = __gcd (ret.a, ret.b);
        ret.a /= d, ret.b /= d;
        return ret;
    }
    Fraction operator*(const Fraction &rhs) const {
        int d1 = __gcd (b, rhs.a);
        int d2 = __gcd (a, rhs.b);
        
        Fraction ret;
        
        ret.b = b / d1 * rhs.b / d2;
        ret.a = a / d1 * rhs.a / d2;
        return ret;
    }
}
```