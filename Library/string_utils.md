```cpp
void removeWhiteSpace(string &s) {
    s.erase(remove_if(ALL(s), [](char x) {
        return isspace(x);
    }), s.end());
}
```

```cpp
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

vector<string> split(string const& s) {
    regex regex{R"([\s,]+)"}; // split on space and comma
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}
```
join vector to form a string, works for both string and char
```cpp
template<typename T>
string join(vector<T> const& v, string delim="") {
    string s;
    bool first = true;
    for (auto const& piece: v) {
        if(first) first = false;
        else s += delim;
        s += piece;
    }
    return s;
}
```