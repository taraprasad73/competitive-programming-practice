```cpp
void solve() {
    int n, k; cin >> n >> k;
    map<string, set<int>> cnt;
    set<string> used;
    for(int i = 0; i < k; ++i) {
        int a, b; string x, y;
        cin >> a >> b >> x >> y;
        cnt[x].insert(a);
        cnt[y].insert(b);
        if(x == y) {
            used.insert(x); 
        }
    }
    int ans = 0, singles = 0, unknowns = n;
    for(auto it = cnt.begin(); it != cnt.end(); it++) {
        string key = it->first;
        int size = it->second.size();
        unknowns -= size;
        if(size == 1)
            ++singles;
        else if(not CONTAINS(used, key) and size == 2) 
            ++ans;
    }
    int unknownPairs = (unknowns - singles) / 2;
    if(unknownPairs == 0)
        ans += singles;
    else if(unknownPairs == 1 and singles == 0)
        ++ans;
    cout << ans << endl;
}
```