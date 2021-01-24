# Rotating 2D array by 45 degrees, so that the diagoanls determine the new positions
```cpp
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
```