```cpp
const char * zero = 
" - \n"
"| |\n"
"   \n"
"| |\n"
" - \n";

const char * one = 
"   \n"
"  |\n"
"   \n"
"  |\n"
"   \n";

const char * two = 
" - \n"
"  |\n"
" - \n"
"|  \n"
" - \n";

const char * three = 
" - \n"
"  |\n"
" - \n"
"  |\n"
" - \n";

const char * four = 
"   \n"
"| |\n"
" - \n"
"  |\n"
"   \n";

const char * five = 
" - \n"
"|  \n"
" - \n"
"  |\n"
" - \n";

const char * six = 
" - \n"
"|  \n"
" - \n"
"| |\n"
" - \n";

const char * seven = 
" - \n"
"  |\n"
"   \n"
"  |\n"
"   \n";

const char * eight = 
" - \n"
"| |\n"
" - \n"
"| |\n"
" - \n";

const char * nine = 
" - \n"
"| |\n"
" - \n"
"  |\n"
" - \n";

vs scale(int s, vs const& num) {
    vs output(2*s + 3, string(s + 2, ' '));
    for(int i = 0; i < num.size(); ++i) {
        for(int j = 0; j < num[i].size(); ++j) {
            char c = num[i][j];
            if(c == '-') {
                int row = 0;
                if(i == 2) row = s + 1;
                else if(i == 4) row = 2*s + 2;
                for(int x = 1; x < s + 1; ++x) 
                    output[row][x] = '-';
            } else if(c == '|') {
                int row = i == 1 ? 1: s + 2;
                int col = j == 0 ? 0 : s + 1;
                for(int x = row; x < s + row; ++x) 
                    output[x][col] = '|';
            }
        }
    }
    return output;
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
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}
vector<string> split(string const& s, string delim, uint parts=0) {
    vector<string> tokens = split(s, regex{"([" + delim + "])"});
    if(parts != 0 and tokens.size() >= parts) {
        tokens[parts - 1] = join(vector<string>(tokens.begin() + parts - 1, tokens.end()), delim);
        tokens.erase(tokens.begin() + parts, tokens.end());
    }
    return tokens;
}

vector<string> join(vector<vector<string>> const& units, string delim="") {
    vector<string> output(units[0].size());
    for(int i = 0; i < units[0].size(); ++i) {
        for(int j = 0; j < units.size(); ++j) {
            if(j != 0) 
                output[i] += delim;
            output[i] += units[j][i];
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
    vs codes = {zero, one, two, three, four, five, six, seven, eight, nine};
    vvs code_arrays;
    for(auto &code: codes) {
        code_arrays.pb(split(code, "\n"));
    }
    while(true) {
        int s; string num; cin >> s >> num;
        if(s == 0)
            break;
        vvs digits;
        for(char c: num) {
            digits.pb(scale(s, code_arrays[c - '0']));
        }
        vs answer = join(digits, " ");
        print(answer);
        cout << endl;
    }
}
```