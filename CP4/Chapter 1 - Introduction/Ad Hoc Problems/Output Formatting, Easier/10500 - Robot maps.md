```cpp
vector<ii> get_neighbors(int x, int y) {
    return {{x - 1, y}, {x, y + 1}, {x + 1, y}, {x, y - 1}};
}

int scan(int x, int y, vector<vector<char>> &grid, vector<vector<char>> &output) {
    int movements = 0;
    while(true) {
        if(grid[x][y] == 'V') break;
        grid[x][y] = 'V';
        output[x][y] = '0';
        for(ii p: get_neighbors(x, y)) {
            if(grid[p.F][p.S] != 'V')
                output[p.F][p.S] = grid[p.F][p.S];
        }
        for(ii p: get_neighbors(x, y)) {
            if(grid[p.F][p.S] == 'X' or grid[p.F][p.S] == 'V') {
                continue;
            } else {
                x = p.F;
                y = p.S;
                ++movements;
                break;
            }
        }
    }
    return movements;
}

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

void print(int n, int m, vector<vector<char>> const& output) {
    string separator = "|" + join(vector<string>(m, "---|"));
    cout << separator << endl;
    for(int i = 1; i <= n; ++i) {
        cout << "| " << join(vector<char>(output[i].begin() + 1, output[i].begin() + m + 1), " | ") << " |" << endl;
        cout << separator << endl;
    }
}

void solve() {
    while(true) {
        int n, m;
        cin >> n >> m;
        if(n == 0 and m == 0) break;
        int x, y; cin >> x >> y;
        vector<vector<char>> grid(n + 2, vector<char>(m + 2, 'X'));
        vector<vector<char>> output(n + 2, vector<char>(m + 2, 'X'));
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= m; ++j) {
                cin >> grid[i][j];
                output[i][j] = '?';
            }
        }
        int movements = scan(x, y, grid, output);
        cout << endl;
        print(n, m, output);
        cout << endl;
        cout << "NUMBER OF MOVEMENTS: " << movements << endl;
    }
}
```