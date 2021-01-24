# 2D Max Sum Brute
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
 * a: should be a padded 2d array, with 1 cell padding on all sides. If not, then needsPadding should be true. 
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
T max2DRangeSumBrute(vector<vector<T>> &a, int r, int c, bool needsPadding=false) {
    if(needsPadding)
        a = addPadding(a, r, c);
    rangeSum2DPreProcess(a, r, c);
    T ans = numeric_limits<T>::min();
    for(int r1 = 1; r1 <= r; ++r1) {
        for(int r2 = r1; r2 <= r; ++r2) {
            for(int c1 = 1; c1 <= c; ++c1) {
                for(int c2 = c1; c2 <= c; ++c2) {
                    ans = max(rangeSum2D(a, r1, c1, r2, c2), ans);
                }
            }
        }
    }
    return ans;
}

/**
 * Variant with extra parameters.
 * 
 * maxRows: maximum number of allowed rows of the sub-rectangles 
 * maxCols: maximum number of allowed columns of the sub-rectangles 
 * 
 * a: should be a padded 2d array, with 1 cell padding on all sides. If not, then needsPadding should be true. 
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
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
```
# 2D Max Sum Kadane
```cpp
/**
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * r, c: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void applyPrefixSumsOnColumns(vector<vector<T>> &cnt, int r, int c) {
    for(int i = 1; i <= r; ++i) {
        for(int j = 1; j <= c; ++j) {
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
 * 2D-Kadane algorithm
 * 
 * a: should be a padded 2d array, with 1 cell padding on all sides. If not, then needsPadding should be true.
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
 * 
 * works for all negative numbers too
 * -5 -7 -2
 * -3 -4 -1 -> answer = -1
 */
template<typename T>
T max2DRangeSumKadane(vector<vector<T>> &a, int r, int c, bool needsPadding=false) {
    if(needsPadding)
        a = addPadding(a);
    applyPrefixSumsOnColumns(a, r, c);
    T ans = numeric_limits<T>::min();
    for(int r1 = 1; r1 <= r; ++r1) {
        for(int r2 = r1; r2 <= r; ++r2) {
            // Apply 1d-kadane by treating each column 
            // between r1 and r2 as a single number
            vector<T> column_sums(c);
            for(int i = 1; i <= c; ++i) {
                column_sums[i - 1] = a[r2][i] - a[r1 - 1][i];
            }
            ans = max(max1dRangeSum(column_sums), ans);
        }
    }
    return ans;
}
```
# 2D Kadane Variant: Largest contiguous rectangle containing 0 (uva 10074, uva 836)
Convert the non-required numbers to -INF.
```cpp
vvi a(n + 2, vi(m + 2, 0));
for(int i = 1; i <= n; ++i) {
    for(int j = 1; j <= m; ++j) {
        int x; cin >> x;
        a[i][j] = x == 0 ? 1 : -INF32; 
    }
}
cout << max(max2dRangeSum(a, n, m), 0LL) << endl; // as ans can be -INF
```