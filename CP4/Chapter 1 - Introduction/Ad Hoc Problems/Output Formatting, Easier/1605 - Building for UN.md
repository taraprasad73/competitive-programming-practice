```cpp
char getCountry(int i) {
    if(i < 26) {
        return 'a' + i;
    } else {
        i -= 26;
        return 'A' + i;
    }
}

void solve() {
    string s;
    bool first = true;
    while(getline(cin, s)) {
        if(first) {
            first = false;
        } else {
            cout << endl;
        }
        int n;
        sscanf(s.c_str(), "%d", &n);
        cout << n << " " << n << " " << 2 << endl;
        for(int h = 0; h < n; ++h) {
            if(h != 0) cout << endl;
            for(int c = 0; c < n; ++c) {
                cout << getCountry(c);
            }
            cout << endl;
            for(int c = 0; c < n; ++c) {
                cout << getCountry(h);
            }
            cout << endl;
        }
    }
}
```