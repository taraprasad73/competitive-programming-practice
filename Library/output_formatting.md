## Print containters
```cpp
void print(vs const& lines) {
    for(int i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
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