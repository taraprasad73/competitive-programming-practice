```
1 2 3 4 5 6 7 8 9
-1 -1 -1
23 -1 -24 2 23
1 -14 -4 14 -11 -7 6
```
Read each line till EOF and split on space.
```cpp
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