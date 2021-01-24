# Inferior code quality
```cpp
/**
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * r, c: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void rangeSum2DPreProcess(vector<vector<T>> &cnt, int r, int c) {
    for(int i = 1; i <= r; ++i) {
        for(int j = 1; j <= c; ++j) {
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

/**
 * rotate array by 45degrees and enlarge it
 * new position of each cell = index of diagonals crossing through that cell
 * 45 degree diagonals -> new row index -> r + c - 1
 * 135 degree diagonals -> new col index -> c - r + num_rows
 */ 
void solve() {
    int t = 0;
    while(true) {
        ++t;
        int r, c, n, q; cin >> r >> c >> n >> q;
        if(r == 0 && c == 0 && n == 0 && q == 0) break;
        int R = r + c - 1;
        int C = r + c - 1;
        vvi a(R + 2, vi(C + 2, 0));
        for(int i = 0; i < n; ++i) {
            int x, y; cin >> x >> y;
            int X = x + y - 1;
            int Y = y - x + r;
            a[X][Y] = 1;
        }
        rangeSum2DPreProcess(a, R, C);
        cout << "Case " << t << ":" << endl;
        for(int i = 0; i < q; ++i) {
            int d; cin >> d; // distance
            ll max_cnt = -1; int row, col;
            // question seems to have a typo, x loop should be first and should be descending,
            // ... but the answer matches this way
            for(int y = 1; y <= c; ++y) { 
                for(int x = 1; x <= r; ++x) {
                    int X = x + y - 1;
                    int Y = y - x + r;
                    ll cnt = rangeSum2D(a, max(1, X - d), max(1, Y - d), min(R, X + d), min(C, Y + d));
                    if(cnt > max_cnt) {
                        max_cnt = cnt;
                        row = x;
                        col = y;
                    } else if(cnt == max_cnt) {
                        // already handled due to the order of traversal of x and y    
                    }
                }
            }
            cout << max_cnt << " (" << row << "," << col << ")" << endl;
        }
    }
}
```

# Refactored (better code quality)
```cpp
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
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * r, c: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void rangeSum2DPreProcess(vector<vector<T>> &cnt, int r, int c, bool needsPadding=false) {
    if(needsPadding)
        addPadding(cnt, r, c);
    for(int i = 1; i <= r; ++i) {
        for(int j = 1; j <= c; ++j) {
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

/**
 * r: number of rows in original matrix
 * x: row number (1-indexed)
 * y: column number (1-indexed)
 * output: 1-indexed new row and column for the given input
 */ 
tuple<int, int> rotatePosition45Clockwise(int r, int x, int y) {
    return make_tuple(x + y - 1, y - x + r);
}

/**
 * input: a 1-indexed 2D matrix of size (r + 1, c + 1)
 * output: a padded (also 1-indexed) 2D matrix
 */ 
template<typename T>
vector<vector<T>> rotate45Clockwise(vector<vector<T>> &a, int r, int c) {
    int R = r + c - 1, C = r + c - 1;
    vvi ret(R + 2, vi(C + 2, 0));
    for(int i = 1; i <= r; ++i) {
        for(int j = 1; j <= c; ++j) {
            int X, Y; tie(X, Y) = rotatePosition45Clockwise(r, i, j);
            ret[X][Y] = a[i][j];
        }
    }
    return ret;
}

void solve() {
    int t = 0;
    while(true) {
        ++t;
        int r, c, n, q; cin >> r >> c >> n >> q;
        if(r == 0 && c == 0 && n == 0 && q == 0) break;
        vvi a(r + 1, vi(c + 1, 0));
        for(int i = 0; i < n; ++i) {
            int x, y; cin >> x >> y;
            a[x][y] = 1;            
        }
        a = rotate45Clockwise(a, r, c);
        int R = r + c - 1;
        int C = r + c - 1;
        rangeSum2DPreProcess(a, R, C);
        cout << "Case " << t << ":" << endl;
        for(int i = 0; i < q; ++i) {
            int d; cin >> d; // distance
            ll max_cnt = -1; int row, col;
            // question seems to have a typo, x loop should be first and should be descending,
            // ... but the answer matches this way
            for(int y = 1; y <= c; ++y) { 
                for(int x = 1; x <= r; ++x) {
                    int X, Y; tie(X, Y) = rotatePosition45Clockwise(r, x, y);
                    ll cnt = rangeSum2D(a, max(1, X - d), max(1, Y - d), min(R, X + d), min(C, Y + d));
                    if(cnt > max_cnt) {
                        max_cnt = cnt;
                        row = x;
                        col = y;
                    } else if(cnt == max_cnt) {
                        // already handled due to the order of traversal of x and y    
                    }
                }
            }
            cout << max_cnt << " (" << row << "," << col << ")" << endl;
        }
    }
}
```