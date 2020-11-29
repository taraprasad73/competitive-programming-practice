# Simple implementation
```cpp
vector<string> codes = {
    "***   * *** *** * * *** *** *** *** ***",
    "* *   *   *   * * * *   *     * * * * *",
    "* *   * *** *** *** *** ***   * *** ***",
    "* *   * *     *   *   * * *   * * *   *",
    "***   * *** ***   * *** ***   * *** ***"
};

vector<vector<vector<char>>> code_map;

void create_mapping() {
    for(int num = 0; num < 10; ++num) {
        vector<vector<char>> code(5, vector<char>(3));
        for(int i = 0; i < 5; ++i) {
            for(int j = num * 4; j < num * 4 + 3; ++j) {
                code[i][j % 4] = codes[i][j];
            }
        }
        code_map.pb(code);
    }
}

int get_number(vector<vector<char>> const& num) {
    for(int x = 0; x < code_map.size(); ++x) {
        bool equal = true;
        for(int i = 0; i < 5; ++i) {
            for(int j = 0; j < 3; ++j) {
                if(code_map[x][i][j] != num[i][j]) {
                    equal = false;
                }
            }
        }
        if(equal) {
            return x;
        }
    }
    return -1;
}

string parse_input() {
    vector<string> input(5);
    for(int i = 0; i < 5; ++i) {
        getline(cin, input[i]);
    }
    int count = (input[0].length() + 1) / 4;
    string number;
    for(int num = 0; num < count; ++num) {
        vector<vector<char>> code(5, vector<char>(3));
        for(int i = 0; i < 5; ++i) {
            for(int j = num * 4; j < num * 4 + 3; ++j) {
                code[i][j % 4] = input[i][j];
            }
        }
        int ret = get_number(code);
        if(ret == -1) {
            return "*";
        }
        number += to_string(ret);
    }
    return number;
}

void solve() {
    create_mapping();
    string number = parse_input();
    if(number != "*") {
        int x = stoi(number);
        if(x % 6 == 0) {
            cout << "BEER!!" << endl;
        } else {
            cout << "BOOM!!" << endl;
        }
    } else {
        cout << "BOOM!!" << endl;
    }
}
```

# Optimized code with string_utils functions
```cpp
typedef vector<string> vs;
typedef vector<vs> vvs;

vs codes = {
    "***   * *** *** * * *** *** *** *** ***",
    "* *   *   *   * * * *   *     * * * * *",
    "* *   * *** *** *** *** ***   * *** ***",
    "* *   * *     *   *   * * *   * * *   *",
    "***   * *** ***   * *** ***   * *** ***"
};

vvs code_map;

void create_mapping() {
    for(int num = 0; num < 10; ++num) {
        vs code(5, string(3, ' '));
        for(int i = 0; i < 5; ++i) {
            for(int j = num * 4; j < num * 4 + 3; ++j) {
                code[i][j % 4] = codes[i][j];
            }
        }
        code_map.pb(code);
    }
}

int get_number(vs const& num) {
    for(int x = 0; x < code_map.size(); ++x) {
        if(code_map[x] == num)
            return x;
    }
    return -1;
}

vvs split(vs const& input, int unit_width, string delim) {
    int total_width = unit_width + delim.length();
    // add extra delim length to make it perfectly divisible by total_width
    int num_units = (input[0].length() + delim.length()) / total_width; 
    int unit_height = input.size();
    vvs output(num_units);
    for(int k = 0; k < num_units; ++k) {
        vector<string> unit(unit_height, string(unit_width, ' '));
        for(int i = 0; i < unit_height; ++i) {
            for(int j = k * total_width; j < k * total_width + unit_width; ++j) {
                unit[i][j % total_width] = input[i][j];
            }
        }
        output[k] = unit;
    }
    return output;
}

string parse_input() {
    vector<string> input(5);
    for(int i = 0; i < 5; ++i) {
        getline(cin, input[i]);
    }
    vvs digits = split(input, 3, " ");
    string number;
    for(vs digit: digits) {
        int ret = get_number(digit);
        if(ret == -1) {
            return "*";
        }
        number += to_string(ret);
    }
    return number;
}

void solve() {
    create_mapping();
    string number = parse_input();
    if(number != "*") {
        int x = stoi(number);
        if(x % 6 == 0) {
            cout << "BEER!!" << endl;
        } else {
            cout << "BOOM!!" << endl;
        }
    } else {
        cout << "BOOM!!" << endl;
    }
}
```