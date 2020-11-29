```cpp
vs join(vvs const& units, string delim="") {
    vs output(units[0].size());
    for(int i = 0; i < units[0].size(); ++i) {
        for(int j = 0; j < units.size(); ++j) {
            if(j != 0) 
                output[i] += delim;
            output[i] += units[j][i];
        }
    }
    return output;
}

vs enlarge(char c, int rows, int cols) {
    return vector<string>(rows, string(cols, c));
}

void print(vs const& lines) {
    for(int i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
}

void solve() {
    int R, C, X, Y; cin >> R >> C >> X >> Y;
    for(int i = 0; i < R; ++i) {
        vvs row;
        for(int j = 0; j < C; ++j) {
            char c; cin >> c;
            vs enlarged = enlarge(c, X, Y);
            row.pb(enlarged);
        }
        vs merged_row = join(row);
        print(merged_row);
    }
}
```