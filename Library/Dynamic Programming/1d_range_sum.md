```cpp
/**
 * kadane algorithm
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
 * kadane algorithm 
 * 
 * Tie breaker: variant 1
 * Criteria 1: smallest range
 * Criteria 2: least start index
 * 
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 * 
 * Returns the smallest range having the maximum sum. If multiple ranges are optimal 
 * having the same lengths, then the first optimal range is returned.
 * range = [start, end] (both inclusive) 
 */
template<typename T>
tuple<T, int, int> max1dRangeSum(vector<T> const& a) {
    T sum = 0, ans = numeric_limits<T>::lowest();
    int start = 0, end = 0, curr_start = 0;
    auto handleTies = [&start, &end](int curr_start, int curr_end) { 
        int last_range_length = end - start + 1;
        int current_range_length = curr_end - curr_start + 1;
        if(current_range_length < last_range_length) {
            end = curr_end;
            start = curr_start;
        }
    };
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        if(sum > ans) {
            ans = sum;
            end = i;
            start = curr_start;
        } else if(sum == ans) {
            handleTies(curr_start, i);
        }
        if(sum <= 0) { // = to exclude 0 valued ranges 
            sum = 0;
            curr_start = i + 1;
        }
    }
    return make_tuple(ans, start, end);
}

/**
 * kadane algorithm: variant 2
 * Tie breaker: 
 * Criteria 1: least start index
 * Criteria 2: smallest range / least end index
 * 
 * Returns the first range having the maximum sum. 
 * If multiple ranges are optimal having the same starting index, 
 * then the first optimal range is returned (with the least length or ending index).
 * range = [start, end] (both inclusive) 
 */ 
template<typename T>
tuple<T, int, int> max1dRangeSum(vector<T> const& a) {
    T sum = 0, ans = numeric_limits<T>::lowest();
    int start = 0, end = 0, curr_start = 0;
    auto handleTies = [&start, &end](int curr_start, int curr_end) { 
        return;
    };
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        if(sum > ans) {
            ans = sum;
            end = i;
            start = curr_start;
        } else if(sum == ans) {
            handleTies(curr_start, i);
        }
        if(sum < 0) { // don't reset curr_start when sum = 0 as we want the least index 
            sum = 0;
            curr_start = i + 1;
        }
    }
    return make_tuple(ans, start, end);
}

/**
 * kadane algorithm 
 * 
 * Tie breaker: variant 1
 * Criteria 1: largest range
 * Criteria 2: least start index
 * 
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 * 
 * Returns the largest range having the maximum sum. If multiple ranges are optimal 
 * having the same lengths, then the first optimal range is returned.
 * range = [start, end] (both inclusive) 
 */
template<typename T>
tuple<T, int, int> max1dRangeSum(vector<T> const& a) {
    T sum = 0, ans = numeric_limits<T>::lowest();
    int start = 0, end = 0, curr_start = 0;
    auto handleTies = [&start, &end](int curr_start, int curr_end) { 
        int last_range_length = end - start + 1;
        int current_range_length = curr_end - curr_start + 1;
        if(current_range_length > last_range_length) {
            end = curr_end;
            start = curr_start;
        }
    };
    for(int i = 0; i < SZ(a); ++i) {
        sum += a[i];
        if(sum > ans) {
            ans = sum;
            end = i;
            start = curr_start;
        } else if(sum == ans) {
            handleTies(curr_start, i);
        }
        if(sum < 0) { // no = to include 0 valued ranges 
            sum = 0;
            curr_start = i + 1;
        }
    }
    return make_tuple(ans, start, end);
}
```