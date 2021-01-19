```cpp
void solve() {
    int t; cin >> t;
    while(t--) {
        int n, m; cin >> n >> m;
        vi a(n);
        vi m_positions;
        for(int i = 0; i < n; ++i) {
            cin >> a[i];
            if(a[i] == m)
                m_positions.pb(i);
        }
        ll ans = 0;
        for(int x: m_positions) {
            ll rSum = 0, maxRSum = 0;
            for(int i = x + 1; i < n and a[i] > m; ++i) {
                rSum += a[i];
                maxRSum = max(rSum, maxRSum);
            }
            ll lSum = 0, maxLSum = 0;
            for(int i = x - 1; i >= 0 and a[i] > m; --i) {
                lSum += a[i];
                maxLSum = max(lSum, maxLSum);
            }
            ans = max(ans, maxLSum + a[x] + maxRSum);
        }
        cout << ans << endl;
    }
}
```