## Print containters
```cpp
void print(vs const& lines, bool prettyPrint = true) {
    if(prettyPrint && !lines.empty())
        cerr << string(lines[0].size(), '-') << endl;
    for(int i = 0; i < lines.size(); ++i) {
        cerr << lines[i] << endl;
    }
    if(prettyPrint && !lines.empty())
        cerr << string(lines[lines.size() - 1].size(), '=') << endl;
    cerr << endl;
}

template<typename T>
void print(vector<vector<T>> const& a, bool prettyPrint = true) {
    if(prettyPrint && !a.empty())
        cerr << string(2 * a[0].size(), '-') << endl;
    for(int i = 0; i < a.size(); ++i) {
        for(int j = 0; j < a[i].size(); ++j) {
            cerr << a[i][j] << " ";
        }
        cerr << endl;
    }
    if(prettyPrint && !a.empty())
        cerr << string(2 * a[a.size() - 1].size(), '=') << endl;
    cerr << endl;
}
```

## Add commas to numbers
https://stackoverflow.com/questions/7276826/c-format-number-with-commas
```cpp
class comma_numpunct : public numpunct<char> {
  protected:
    virtual char do_thousands_sep() const {
        return ',';
    }

    virtual std::string do_grouping() const {
        return "\03";
    }
};

locale comma_locale(locale(), new comma_numpunct());
cerr.imbue(comma_locale);
```