```cpp
int lnds(vi const& a) {
    vi lis;
    for(int i = 0; i < SZ(a); ++i) {
        auto itr = upper_bound(lis.begin(), lis.end(), a[i]);
        if(itr == lis.end()) 
            lis.pb(a[i]);
        else 
            *itr = a[i];
    }
    return lis.size();
}

void solve() {
    while(true) {
        int n; cin >> n;
        if(n == 0) break;
        vii p(n);
        for(int i = 0; i < n; ++i) {
            cin >> p[i].F >> p[i].S;
        }
        sort(ALL(p));
        vi a(n);
        for(int i = 0; i < n; ++i) {
            a[i] = p[i].S;
        }
        cout << lnds(a) << endl;
    }
    cout << "*" << endl;
}
```