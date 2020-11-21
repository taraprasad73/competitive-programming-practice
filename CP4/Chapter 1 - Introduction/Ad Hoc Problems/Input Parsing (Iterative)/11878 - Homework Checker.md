```cpp
void solve() {
    string line;
    int ans = 0;
    while (getline(cin, line)) {
        int a, b, c;
        char op;
        sscanf(line.c_str(), "%d%c%d=%d", &a, &op, &b, &c);
        if(op == '+') {
            if(a + b == c) 
                ++ans;
        } else if(op == '-') {
            if(a - b == c)
                ++ans;
        }
    }
    cout << ans << endl;
}
```