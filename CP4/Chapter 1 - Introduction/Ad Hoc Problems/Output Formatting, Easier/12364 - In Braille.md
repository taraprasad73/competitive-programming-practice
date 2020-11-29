```cpp
typedef vector<string> vs;
typedef vector<vs> vvs;

// split vector<string> into units of vector<vector<string>> by a column delimiter
//{"*. *. ** **",     {{"*.", "..", ".."},
// ".. *. .. ..",  --> {"*.", "*.", ".."},
// ".. .. .. .."}      {"**", "..", ".."},
//                     {"**", "..", ".."}}
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

// merges vector<string> units side by side, separated by a column delim to form vector<vector<string>>
//{{"*.", "..", ".."},         {"*. *. ** **",     
// {"*.", "*.", ".."},   -->    ".. *. .. ..",
// {"**", "..", ".."},          ".. .. .. .."}      
// {"**", "..", ".."}}
vs merge(vvs const& units, string delim) {
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

vvs codes = {
    {".*", "**", ".."}, // 0
    {"*.", "..", ".."}, // 1
    {"*.", "*.", ".."}, // 2
    {"**", "..", ".."}, // 3
    {"**", ".*", ".."}, // 4
    {"*.", ".*", ".."}, // 5
    {"**", "*.", ".."}, // 6
    {"**", "**", ".."}, // 7
    {"*.", "**", ".."}, // 8
    {".*", "*.", ".."}  // 9
};

vs get_lines(int rows) {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    vs input(rows);
    for(int i = 0; i < rows; ++i) {
        getline(cin, input[i]);
    }
    return input;
}

string parse_braille() {
    vs input = get_lines(3);
    vvs digits = split(input, 2, " ");
    string output = "";
    for(vs digit: digits) {
        for(int i = 0; i < codes.size(); ++i) {
            if(digit == codes[i]) {
                output += to_string(i);
                break;
            } 
        }
    }
    return output;
} 

void print(vs const& lines) {
    for(int i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
}

void solve() {
    while(true) {
        int D; cin >> D;
        if(D == 0) break;
        char c; cin >> c;
        if(c == 'S') {
            string digits; cin >> digits;
            vvs braille_output(D);
            for(int i = 0; i < D; ++i) {
                braille_output[i] = codes[digits[i] - '0'];
            }
            vs merged_codes = merge(braille_output, " ");
            print(merged_codes);
        } else {
            cout << parse_braille() << endl;
        }
    }
}
```