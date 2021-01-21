```cpp
/**
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void rangeSum2DPreProcess(vector<vector<T>> &cnt, int rows, int columns) {
    for(int i = 1; i <= rows; ++i) {
        for(int j = 1; j <= columns; ++j) {
            cnt[i][j] += cnt[i - 1][j];
            cnt[i][j] += cnt[i][j - 1];
            cnt[i][j] -= cnt[i - 1][j - 1];
        }
    }
}

/**
 * cnt -> 2D array on which max2dSumPreProcess() has already been called
 * r1, c1 -> North-West corner cell
 * r2, c2 -> South-East corner cell
 */ 
template<typename T>
T rangeSum2D(vector<vector<T>> const& cnt, int r1, int c1, int r2, int c2) {
    T sum = cnt[r2][c2];
    sum -= cnt[r2][c1 - 1];
    sum -= cnt[r1 - 1][c2];
    sum += cnt[r1 - 1][c1 - 1];
    return sum;
}

void solve() {
    bool first = true;
    while (true) {
        string line;
        while(line.empty()) {
            if(!getline(cin, line))
                exit(0); // EOF
        }
        if(first) {
            first = false;
        } else {
            cout << endl;
        }
        istringstream line_stream(line);
        int n, m; line_stream >> n >> m; wi(n, m)
        vvi a(n + 2, vi(n + 2));
        for(int i = n; i >= 1; --i) {
            for(int j = 1; j <= n; ++j) {
                cin >> a[i][j];
            }
        }
        rangeSum2DPreProcess(a, n, n);
        ll sum = 0;
        for(int i = n - m + 1; i >= 1; --i) {
            for(int j = 1; j <= n - m + 1; ++j) {
                ll x = rangeSum2D(a, i, j, i + m - 1, j + m - 1);
                sum += x;
                cout << x << endl;
            }
        }
        cout << sum << endl;
    }
}
```