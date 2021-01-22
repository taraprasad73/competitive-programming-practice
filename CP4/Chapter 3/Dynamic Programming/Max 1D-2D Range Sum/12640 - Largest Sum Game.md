```cpp
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
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

vector<string> split(string const& s) {
    regex regex{R"([\s]+)"}; // split on space and comma
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

void solve() {
    string line;
    while (getline(cin, line)) {
        vector<string> nums = split(line);
        int n = SZ(nums);
        vi a(n);
        for(int i = 0; i < n; ++i) {
            a[i] = stoi(nums[i]);
        }
        cout << max(max1dRangeSum(a), 0LL) << endl;
    }
}
```