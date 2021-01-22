```cpp
template<typename T>
T max1dRangeSum(vector<T> const& a) {
    T sum = 0, ans = numeric_limits<T>::min();
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        ans = max(ans, sum);
        if(sum < 0)
            sum = 0;
    }
    return ans;
}

void solve() {
    while(true) {
        int n; cin >> n;
        if(n == 0) break;
        vi a(n);
        for(int i = 0; i < n; ++i) {
            cin >> a[i];
        }
        ll ans = max1dRangeSum(a);
        if(ans <= 0) {
            cout << "Losing streak." << endl;
        } else {
            cout << "The maximum winning streak is " << ans << "." << endl;
        }
    }
}
```