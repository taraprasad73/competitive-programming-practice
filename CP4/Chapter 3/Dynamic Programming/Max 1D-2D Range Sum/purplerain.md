```cpp
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
tuple<T, int, int> max1dSum(vector<T> const& a) {
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
 * Special Cases:
 * B
 * R
 * RB -> 1 1 (not 2 2)
 * BR -> 1 1 (not 2 2)
 * BRBBRBRBBRRRBBRBB -> to test tie breaker (when diff = 3)
 */
void solve() {
    string s; cin >> s;
    vi a(SZ(s)), b(SZ(s));
    for(int i = 0; i < SZ(s); ++i) {
        if(s[i] == 'B') {
            a[i] = -1;
            b[i] = 1;
        } else {
            a[i] = 1;
            b[i] = -1;
        }
    }
    ll ans1, start1, end1;
    tie(ans1, start1, end1) = max1dSum(a);
    ll ans2, start2, end2;
    tie(ans2, start2, end2) = max1dSum(b);
    if(ans1 > ans2) {
        cout << start1 + 1 << " " << end1 + 1 << endl;
    } else if(ans1 == ans2) {
        if(start1 < start2)
            cout << start1 + 1 << " " << end1 + 1 << endl;
        else
            cout << start2 + 1 << " " << end2 + 1 << endl;
    } else {
        cout << start2 + 1 << " " << end2 + 1 << endl;
    }
}
```