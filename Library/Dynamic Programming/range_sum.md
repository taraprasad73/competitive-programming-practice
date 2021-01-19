```cpp
/**
 * kadane algorithm
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 */
template<typename T>
T max1dSum(vector<T> const& a) {
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
 * kadane algorithm
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 * 
 * Returns the smallest range having the maximum sum. If multiple ranges are optimal 
 * having the same lengths, then the first optimal range is returned.
 * range = [start, end] (both inclusive) 
 */
template<typename T>
tuple<T, int, int> max1dSum(vector<T> const& a) {
    T sum = 0, ans = numeric_limits<T>::lowest();
    int start = 0, end = 0, curr_start = 0;
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        if(sum > ans) {
            ans = sum;
            end = i;
            start = curr_start;
        } else if(sum == ans) {
            int last_range_length = end - start + 1;
            int current_range_length = i - curr_start + 1;
            if(current_range_length < last_range_length) {
                end = i;
                start = curr_start;
            }
        }
        if(sum <= 0) { // = to exclude 0 valued ranges 
            sum = 0;
            curr_start = i + 1;
        }
    }
    return make_tuple(ans, start, end);
}
```