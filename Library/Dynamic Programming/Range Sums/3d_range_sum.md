```cpp
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
 * Requires padding of 1 cell on each surface.
 */ 
template<typename T>
void rangeSum3DPreProcess(vector<vector<vector<T>>> &cnt, int r, int c, int h) {
    // preprocess each 2 dimensional sheet
    for(int i = 1; i <= h; ++i) {
        rangeSum2DPreProcess(cnt[i], r, c);
    }
}

/**
 * Requires padding of 1 cell on each surface.
 * array should be stored in h x r x c format
 */ 
template<typename T>
T max3DRangeSumKadane(vector<vector<vector<T>>> &a, int r, int c, int h) {
    rangeSum3DPreProcess(a, r, c, h);
    T ans = numeric_limits<T>::min();
    for(int r1 = 1; r1 <= r; ++r1) {
        for(int r2 = r1; r2 <= r; ++r2) {
            for(int c1 = 1; c1 <= c; ++c1) {
                for(int c2 = c1; c2 <= c; ++c2) {
                    // Apply 1d-kadane by treating each reactangle as 1 unit 
                    vector<T> rectangle_sums(h);
                    for(int i = 1; i <= h; ++i) {
                        rectangle_sums[i - 1] = rangeSum2D(a[i], r1, c1, r2, c2);
                    }
                    ans = max(max1dRangeSum(rectangle_sums), ans);
                }
            }
        }
    }
    return ans;
}
```