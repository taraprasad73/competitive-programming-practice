Hint: Remember there's no unary operator in input, so 2x=-1 is not a valid input. Try to find the total sum of all coefficients of x and the total sum of all constants.
```cpp
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

int get_coeff(string s) {
    if(s.length() == 1) 
        return 1;
    else {    
        string coeff = s.substr(0, s.size() - 1);
        return stoi(coeff);
    }
}

ii get_values(string expression) {
    int constant_sum = 0, coefficient_sum = 0;
    vector<string> tokens = split(expression, regex{R"([+])"});
    for(string token: tokens) {
        vector<string> s = split(token, regex{R"([-])"});
        // s[0] -> +ve
        if(s[0].find('x') != string::npos) {
            coefficient_sum += get_coeff(s[0]);
        } else {
            constant_sum += stoi(s[0]);
        }
        // all of them -> -ve
        for(int i = 1; i < s.size(); ++i) {
            if(s[i].find('x') != string::npos) {
                coefficient_sum -= get_coeff(s[i]);
            } else {
                constant_sum -= stoi(s[i]);
            }    
        }
    }
    return {constant_sum, coefficient_sum};
}

void solve() {
    int t; cin >> t;
    while(t--) {
        string eq; cin >> eq;
        vector<string> parts = split(eq, regex{R"([=])"});
        ii lhs_values = get_values(parts[0]);
        ii rhs_values = get_values(parts[1]);
        int constant_sum = rhs_values.F - lhs_values.F;
        int coefficient_sum = lhs_values.S - rhs_values.S;
        if(coefficient_sum == 0) {
            if(constant_sum == 0) {
                cout << "IDENTITY" << endl;
            } else {
                cout << "IMPOSSIBLE" << endl;
            }
        } else {
            cout << int(floor(constant_sum / double(coefficient_sum))) << endl; 
        }
    }
}
```