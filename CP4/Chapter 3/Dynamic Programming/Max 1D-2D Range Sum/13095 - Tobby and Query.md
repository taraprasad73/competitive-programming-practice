```cpp
/**
 * Store the positions of each number.
 * Choices: 
 * Set -> range query can be answered by finding the lowerbound of l and r
 * Array -> store 1 at each position, and then compute prefix sum
 */ 
void solve() {
    while(true) {
        string line;
        while(line.empty()) { // gobble up new lines till a non-empty input
            if(!getline(cin, line))
                exit(0); // EOF
        }
        istringstream line_stream(line);
        int n = stoi(line);
        vi a(n + 1);
        vvi cnt(10, vi(n + 1, 0));
        for(int i = 1; i <= n; ++i) {
            cin >> a[i];
            cnt[a[i]][i] = 1;
        }
        for(int i = 0; i < 10; ++i) {
            for(int j = 1; j <= n; ++j) {
                cnt[i][j] += cnt[i][j - 1];
            }
        }
        int q; cin >> q;
        while(q--) {
            int l, r; cin >> l >> r;
            int ans = 0;
            for(int i = 0; i < 10; ++i) {
                if((cnt[i][r] - cnt[i][l - 1]) > 0)
                    ++ans;
            }
            cout << ans << endl;
        }
    }
}
```