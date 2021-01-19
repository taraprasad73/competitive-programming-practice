```cpp
/**
 * kadane algorithm
 * works for all negative numbers too
 * -5 -7 -2 -3 -4 -1 -9 -> answer = -1
 * 
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
        if(sum <= 0) { // = 0 for excluding 0 valued transactions
            sum = 0;
            curr_start = i + 1;
        }
    }
    return make_tuple(ans, start, end);
}

/**
 * Edge Cases:
 * 1
 * 0 0.09 // transaction time can be 0
 * 2
 * 0 0.08 // 0 valued transactions shouldn't be picked in the range
 * 1 0.09
 */ 
void solve() {
    while(true) {
        int n; cin >> n; if(n == 0) break;
        vector<double> a;
        vi transaction_times(n);
        int last_transaction_time = -1; // edge case: 0 0.5 (transaction time = 0)
        for(int i = 0; i < n; ++i) {
            int time; cin >> time;
            transaction_times[i] = time;
            double price; cin >> price;
            double running_cost = -0.08 * (time - last_transaction_time - 1); // excluding the boundaries
            a.push_back(running_cost);
            a.push_back(price - 0.08); // including the running_cost
            last_transaction_time = time;
        }
        wi(a)
        double ans; int start, end;
        tie(ans, start, end) = max1dSum(a);
        start = start / 2;
        end = end / 2;
        if(ans > 0)
            cout << fixed << setprecision(2) << ans << " " << transaction_times[start] << " " << transaction_times[end] << endl;
        else 
            cout << "no profit" << endl;
    }
}
```