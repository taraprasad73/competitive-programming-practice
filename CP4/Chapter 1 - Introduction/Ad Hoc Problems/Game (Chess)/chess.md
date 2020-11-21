```cpp
bool isValid(int x, int y) {
    return x >= 0 and y >= 0 and x < 8 and y < 8;
}

vi dr = {1, 1, -1, -1};
vi dc = {1, -1, 1, -1};

void solve() {
    int t; cin >> t;
    while(t--) {
        char col1; cin >> col1; int c1 = col1 - 'A';
        int r1; cin >> r1; --r1;
        char col2; cin >> col2; int c2 = col2 - 'A';
        int r2; cin >> r2; --r2;
        if(r1 == r2 and c1 == c2) {
            cout << 0 << " " << col1 << " " << r1 + 1 << endl;
        } else if((r1 + c1) % 2 != (r2 + c2) % 2) {
            cout << "Impossible" << endl;
        } else {
            if(abs(r1 - r2) == abs(c1 - c2)) {
                cout << 1 << " " << col1 << " " << r1 + 1 << " " << col2 << " " << r2 + 1 << endl;
            } else {
                cout << 2 << " " << col1 << " " << r1 + 1 << " ";
                set<ii> field;
                for(int i = 0; i < 4; ++i) {
                    for(int r = r1, c = c1; isValid(r, c); r += dr[i], c += dc[i]) {
                        field.insert({r, c});
                    }
                }
                bool found = false;
                for(int i = 0; i < 4; ++i) {
                    if(found) break;
                    for(int r = r2, c = c2; isValid(r, c); r += dr[i], c += dc[i]) {
                        if(CONTAINS(field, ii(r, c))) {
                            char col = c + 'A';
                            cout << col << " " << r + 1 << " " << col2 << " " << r2 + 1 << endl;
                            found = true;
                            break;
                        }
                    }
                }
            }
        }
    }
}
```