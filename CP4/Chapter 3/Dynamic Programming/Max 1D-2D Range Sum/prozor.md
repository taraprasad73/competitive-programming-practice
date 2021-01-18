```cpp
void print(vs const& lines) {
    for(int i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
}

void solve() {
    int R, S, K; cin >> R >> S >> K;
    vector<string> window(R);
    for(int i = 0; i < R; ++i) {
        cin >> window[i];
    }
    vvi cnt(R + 2, vi(S + 2, 0));
    for(int i = 0; i < R; ++i) {
        for(int j = 0; j < S; ++j) {
            if(window[i][j] == '*')
                cnt[i + 1][j + 1] = 1;
        }
    }
    for(int i = 1; i <= R; ++i) {
        for(int j = 1; j <= S; ++j) {
            cnt[i][j] += cnt[i - 1][j];
            cnt[i][j] += cnt[i][j - 1];
            cnt[i][j] -= cnt[i - 1][j - 1];
        }
    }
    ll ans = 0;
    int row, col;
    for(int i = 1; i <= (R - K + 1); ++i) {
        for(int j = 1; j <= (S - K + 1); ++j) {
            // calculate the sum between K x K sub-rectangle, excluding the borders
            ll sum = cnt[i + K - 2][j + K - 2];
            sum -= cnt[i + K - 2][j];
            sum -= cnt[i][j + K - 2];
            sum += cnt[i][j];
            if(sum > ans) {
                ans = sum;
                row = i - 1; // 0 indexed
                col = j - 1; // 0 indexed
            }
        }
    }
    window[row][col] = '+';
    window[row + K - 1][col + K - 1] = '+';
    window[row][col + K - 1] = '+';
    window[row + K - 1][col] = '+';
    for(int i = row + 1; i <= row + K - 2; ++i) {
        window[i][col] = '|';
        window[i][col + K - 1] = '|';
    }
    for(int i = col + 1; i <= col + K - 2; ++i) {
        window[row][i] = '-';
        window[row + K - 1][i] = '-';
    }
    cout << ans << endl;
    print(window);
}
```