```
7
0 2 3 3 7 5 2
3
1 1
2 4
2 7
5
7 7 7 7 7
2
4 5
1 5
```
```cpp
while(true) {
    string line;
    while(line.empty()) { // gobble up new lines till a non-empty input
        if(!getline(cin, line))
            exit(0); // EOF
    }
    istringstream line_stream(line);
    int n = stoi(line);
    vi a(n + 1);
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
    }
    int q; cin >> q;
    while(q--) {
        int l, r; cin >> l >> r;
    }
}
```