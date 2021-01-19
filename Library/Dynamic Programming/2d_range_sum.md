```cpp
/**
 * cnt should be a padded 2d array, with 1 cell padding on all sides. 
 * rows, columns: count of rows and columns of the actual array (excluding the padding)
 */ 
template<typename T>
void max2dSumPreProcess(vector<vector<T>> &cnt, int rows, int columns) {
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
```