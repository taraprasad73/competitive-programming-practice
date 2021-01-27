```cpp
const int MAX_N = 1E7;

vb primes(MAX_N + 1, true);
void sieve(int n) {
    primes[0] = false;
    primes[1] = false;
    for (int p = 2; p*p <= n; p++) {
        if (primes[p] == true) {
            for (int i = p * 2; i <= n; i += p)
                primes[i] = false;
        }
    }
}
```