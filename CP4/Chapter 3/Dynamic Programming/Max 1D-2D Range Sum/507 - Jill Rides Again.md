```cpp
/*
 * Problem - 507
 * Author - TaraPrasad
 * Date - Sep 15, 2016
 * Time - 3:24:10 PM
 */

#include <bits/stdc++.h>
using namespace std;
const int epsilon = 1E-12;
const int INF = 1000000000;
#define DEBUG

vector<pair<int, int> > sols;

pair<int, int> max_1d(vector<int> a) {
    int start = 0, end = -1;
    int curr_start = 0;
    int sum = 0, ans = 0;
    for (int i = 0; i < (int) a.size(); ++i) {
        sum += a[i];
        if (sum > ans) {
            ans = sum;
            start = curr_start;
            end = i + 1;
        } else if (sum == ans) {
            if (ans != 0 && i - curr_start > end - start) {
                start = curr_start;
                end = i + 1;
            } else if (ans != 0 && start == curr_start) {
                end = i + 1;
            }
        }
        if (sum < 0) {
            sum = 0;
            curr_start = i + 1;
            //cout << "curr_start = " << curr_start << endl;
        }
    }
    return make_pair(start, end);
}

int main() {
    //std::ios::sync_with_stdio(false);
#ifdef DEBUG
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    int num_routes;
    cin >> num_routes;
    for (int i = 0; i < num_routes; i++) {
        int stops;
        cin >> stops;
        vector<int> road_quality(stops - 1);
        for (int j = 0; j < stops - 1; ++j) {
            cin >> road_quality[j];
        }
        int a = max_1d(road_quality).first;
        int b = max_1d(road_quality).second;
        if (a < b) {
            printf("The nicest part of route %d is between stops %d and %d\n",
                    i + 1, a + 1, b + 1);
        } else {
            printf("Route %d has no nice parts\n", i + 1);
        }
    }
    return 0;
}
```

```cpp
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

void solve() {
    int t; cin >> t;
    for(int j = 1; j <= t; ++j) {
        int n; cin >> n;
        vi a(n - 1);
        for(int i = 0; i < n - 1; ++i) {
            cin >> a[i];
        }
        ll sum, start, end;
        tie(sum, start, end) = max1dRangeSum(a);
        if(sum <= 0) {
            cout << "Route " << j << " has no nice parts" << endl;
        } else {
            cout << "The nicest part of route " << j << " is between stops " << start + 1 << " and " << end + 2 << endl;
        }
    }
}
```