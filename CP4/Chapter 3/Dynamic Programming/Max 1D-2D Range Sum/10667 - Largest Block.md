```cpp
/**
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void max2dRangeSumPreProcess(vector<vector<T>> &cnt, int rows, int columns) {
    for(int i = 1; i <= rows; ++i) {
        for(int j = 1; j <= columns; ++j) {
            cnt[i][j] += cnt[i - 1][j];
        }
    }
}

/**
 * 1D-Kadane algorithm
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 */
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

/**
 * 2D-Kadane algorithm
 * 
 * works for all negative numbers too
 * -5 -7 -2
 * -3 -4 -1 -> answer = -1
 */
template<typename T>
T max2dRangeSum(vector<vector<T>> &a, int rows, int columns) {
    max2dRangeSumPreProcess(a, rows, columns);
    T ans = numeric_limits<T>::min();
    for(int start_row = 1; start_row <= rows; ++start_row) {
        for(int end_row = start_row; end_row <= rows; ++end_row) {
            // Apply 1d-kadane by treating each column 
            // between start_row and end_row as a single number
            vector<T> column_sums(columns);
            for(int i = 1; i <= columns; ++i) {
                column_sums[i - 1] = a[end_row][i] - a[start_row - 1][i];
            }
            ans = max(max1dRangeSum(column_sums), ans);
        }
    }
    return ans;
}
void solve() {
    int t; cin >> t;
    while(t--) {
        int s; cin >> s;
        vvi board(s + 2, vi(s + 2, 0));
        for(int i = 1; i <= s; ++i) {
            for(int j = 1; j <= s; ++j) {
                board[i][j] = 1;
            }
        }
        int n; cin >> n;
        for(int i = 0; i < n; ++i) {
            int r1, c1, r2, c2; 
            cin >> r1 >> c1 >> r2 >> c2;
            for(int x = r1; x <= r2; ++x) {
                for(int y = c1; y <= c2; ++y) {
                    board[x][y] = -INF32;
                }
            }
        }
        cout << max(max2dRangeSum(board, s, s), 0LL) << endl;
    }
}
```