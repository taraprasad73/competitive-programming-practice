# ModInt Template
This is the ModInt template used for the C problem. New additions include prefix and postix operators for both increment and decrement operations. 
```cpp
template<int MOD>
struct ModInt {
    int x;
    ModInt() : x(0) {}
    ModInt(int _x) {
        x = _x % MOD;
        if (x < 0) x += MOD;
    }
    ModInt(long long _x) {
        x = _x % MOD;
        if (x < 0) x += MOD;
    }
    ModInt& operator+=(ModInt rhs) {
        x += rhs.x;
        if (x >= MOD) x -= MOD;
        return *this;
    }
    ModInt& operator-=(ModInt rhs) {
        x -= rhs.x;
        if (x < 0) x += MOD;
        return *this;
    }
    ModInt& operator*=(ModInt rhs) {
        x = (long long)x * rhs.x % MOD;
        return *this;
    }
    ModInt& operator/=(ModInt rhs) {
        return *this *= rhs.inv();
    }
    ModInt operator+(ModInt rhs) const { return ModInt(*this) += rhs; }
    ModInt operator-(ModInt rhs) const { return ModInt(*this) -= rhs; }
    ModInt operator*(ModInt rhs) const { return ModInt(*this) *= rhs; }
    ModInt operator/(ModInt rhs) const { return ModInt(*this) /= rhs; }
    ModInt& operator++() { return *this += ModInt(1); }     // prefix
    ModInt& operator--() { return *this -= ModInt(1); }     // prefix
    ModInt operator++(int) { return *this += ModInt(1); }   // postfix
    ModInt operator--(int) { return *this -= ModInt(1); }   // postfix

    ModInt inv() const {
        // should work for non-prime MOD if gcd(x, MOD) = 1
        int a = x, b = MOD, u = 1, v = 0;
        while (b != 0) {
            int t = a / b;
            a -= t * b;
            u -= t * v;
            swap(a, b);
            swap(u, v);
        }
        return u;
    }
    ModInt pow(long long e) {
        ModInt r = 1, p = *this;
        while (e) {
            if (e & 1) r *= p;
            p *= p;
            e >>= 1;
        }
        return r;
    }
    bool operator==(ModInt rhs) { return x == rhs.x; }
    bool operator!=(ModInt rhs) { return x != rhs.x; }
    bool operator<(ModInt rhs) { return x < rhs.x; }
    bool operator<=(ModInt rhs) { return x <= rhs.x; }
    bool operator>(ModInt rhs) { return x > rhs.x; }
    bool operator>=(ModInt rhs) { return x >= rhs.x; }
    friend ostream& operator<<(ostream &os, ModInt i) { return os << i.x; }
};
```