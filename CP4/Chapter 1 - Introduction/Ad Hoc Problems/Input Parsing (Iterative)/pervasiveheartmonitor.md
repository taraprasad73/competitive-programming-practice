```cpp
void solve() {
    cout << fixed << setprecision(6);
    string s;
    while(getline(cin, s)) {
        vector<string> words = split(s, regex{R"([\s]+)"});
        string name;
        double sum = 0;
        int count = 0;
        for(string word: words) {
            if(isalpha(word.at(0))) {
                name += word + " ";
            } else {
                sum += stod(word);
                ++count;
            }
        }
        cout << sum/count << " " << name << endl;
    }
}
```