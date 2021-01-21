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

template<typename T>
vector<vector<T>> addPadding(vector<vector<T>> const& a, int r, int c) {
    vector<vector<T>> b(r + 2, vector<T>(c + 2, 0));
    for(int i = 0; i < r; ++i) {
        for(int j = 0; j < c; ++j) {
            b[i + 1][j + 1] = a[i][j];
        }
    }
    return b;
}

/**
 * maxRows: maximum number of allowed rows of the sub-rectangles 
 * maxCols: maximum number of allowed columns of the sub-rectangles 
 */ 
template<typename T>
T max2DRangeSumBrute(vector<vector<T>> &a, int r, int c, int maxRows, int maxCols, bool needsPadding=false) {
    if(needsPadding)
        a = addPadding(a, r, c);
    rangeSum2DPreProcess(a, r, c);
    T ans = numeric_limits<T>::min();
    for(int r1 = 1; r1 <= r; ++r1) {
        for(int r2 = r1; r2 <= r && r2 < r1 + maxRows; ++r2) {
            for(int c1 = 1; c1 <= c; ++c1) {
                for(int c2 = c1; c2 <= c && c2 < c1 + maxCols; ++c2) {
                    ans = max(rangeSum2D(a, r1, c1, r2, c2), ans);
                }
            }
        }
    }
    return ans;
}

/**
 * Special cases: (when solution with both horizontal and vertical wrapping exists)
3
 1 -1  1
-1 -1 -1
 1 -1  1
Ans = 4 not 2
(easy to visualize that the corners can wrap)
 1 -1  1  1 -1  1
-1 -1 -1 -1 -1 -1
 1 -1  1  1 -1  1
 1 -1  1  1 -1  1
-1 -1 -1 -1 -1 -1
 1 -1  1  1 -1  1
----------------------------------------------
4 
1  0  0 1 
0 -1 -1 0 
0  4 -1 0 
1  0  0 2 
Ans = 9 not 8
(not easy to visualize the solution 9)
1  0  0 1 1  0  0 1 
0 -1 -1 0 0 -1 -1 0 
0  4 -1 0 0  4 -1 0 
1  0  0 2 1  0  0 2
1  0  0 1 1  0  0 1 
0 -1 -1 0 0 -1 -1 0 
0  4 -1 0 0  4 -1 0 
1  0  0 2 1  0  0 2
 */ 
void solve() {
    int t; cin >> t;
    while(t--) {
        int n; cin >> n;
        vvi a(2 * n, vi(2 * n, 0));
        for(int i = 0; i < n; ++i) {
            for(int j = 0; j < n; ++j) {
                cin >> a[i][j];
                a[i + n][j] = a[i][j];
                a[i][j + n] = a[i][j];
                a[i + n][j + n] = a[i][j];
            }
        }
        cout << max2DRangeSumBrute(a, 2 * n, 2 * n, n, n, true) << endl;
    }
}
```