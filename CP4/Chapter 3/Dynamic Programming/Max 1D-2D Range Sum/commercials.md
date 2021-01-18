```cpp
/**
 * kadane algorithm
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 */
ll max1dSum(vi const& a) {
    ll sum = 0, ans = -INF64;
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        ans = max(ans, sum);
        if(sum < 0)
            sum = 0;
    }
    return ans;
}

void solve() {
    int n, p; cin >> n >> p;
    vi a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
        a[i] -= p;
    }
    cout << max1dSum(a) << endl;
}
```