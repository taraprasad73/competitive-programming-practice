```cpp
map<char, int> order = {{'P', 0}, {'N', 1}, {'B', 2}, {'R', 3}, {'Q', 4}, {'K', 5}};

bool cmp_white(string const& a, string const& b) {
    if(a[0] != b[0])
        return order[a[0]] > order[b[0]];
    if(a[2] != b[2])
        return a[2] < b[2];
    return a[1] < b[1];
}

bool cmp_black(string const& a, string const& b) {
    if(a[0] != b[0])
        return order[a[0]] > order[b[0]];
    if(a[2] != b[2])
        return a[2] > b[2];
    return a[1] < b[1];
}

void solve() {
    vector<vector<char>> board(8, vector<char>(8));
    for(int i = 0; i < 8; ++i) {
        string temp; cin >> temp;
        cin >> temp;
        int index = -1;
        for(auto it = temp.begin(); it != temp.end(); it++) {
            auto c = *it;
            if(c == '|') {
                ++index;
            } else if(isalnum(c)) {
                board[i][index] = c;
            }
        }
    }
    vector<string> white, black;
    for(int i = 0; i < 8; ++i) {
        for(int j = 0; j < 8; ++j) {
            char c = board[i][j];
            char column = 'a' + j;
            char row = '0' +  (8 - i);
            string pos = string(1, column) + string(1, row);
            if(islower(c)) {
                c = toupper(c);
                black.pb(string(1, c) + pos);
            } else if(isupper(c)) {
                white.pb(string(1, c) + pos);
            }
        }
    }
    sort(ALL(white), cmp_white);
    sort(ALL(black), cmp_black);
    cout << "White: ";
    for(int i = 0; i < white.size(); ++i) {
        if(white[i].at(0) == 'P')
            white[i] = white[i].substr(1, 2);
        cout << white[i];
        if(i != white.size() - 1)
            cout << ',';
    }
    cout << endl << "Black: ";
    for(int i = 0; i < black.size(); ++i) {
        if(black[i].at(0) == 'P')
            black[i] = black[i].substr(1, 2);
        cout << black[i];
        if(i != black.size() - 1)
            cout << ',';
    }
}
```