```cpp
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

void solve() {
    string s; cin >> s;
    vector<string> words = split(s, regex{R"([-])"});
    for(string word: words) {
        cout << word.at(0);
    }
}
```