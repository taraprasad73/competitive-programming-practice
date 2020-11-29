```cpp
// use Ctrl + Alt + Down to create multiple cursors to extract the codes from the sample output
//     +  +---+     +   +  +---+
//     |  |         |   |      |
//     |  |      o  |   |      |
//     +  +---+     +---+      +
//     |  |   |  o      |      |
//     |  |   |         |      |
//     +  +---+         +      +
// +---+  +---+     +---+  +---+
//     |      |     |      |   |
//     |      |  o  |      |   |
// +---+  +---+     +---+  +---+
// |          |  o      |      |
// |          |         |      |
// +---+  +---+     +---+  +---+

// +---+  +---+     +---+  +---+
// |   |  |   |     |   |  |   |
// |   |  |   |  o  |   |  |   |
// +   +  +   +     +   +  +---+
// |   |  |   |  o  |   |  |   |
// |   |  |   |     |   |  |   |
// +---+  +---+     +---+  +---+
// the below codes have new lines at the beginning which needs to be removed
string zero = R""""(
+---+
|   |
|   |
+   +
|   |
|   |
+---+
)"""";
string one = R""""(
    +
    |
    |
    +
    |
    |
    +
)"""";
string two = R""""(
+---+
    |
    |
+---+
|    
|    
+---+
)"""";
string three = R""""(
+---+
    |
    |
+---+
    |
    |
+---+
)"""";
string four = R""""(
+   +
|   |
|   |
+---+
    |
    |
    +
)"""";
string five = R""""(
+---+
|    
|    
+---+
    |
    |
+---+
)"""";
string six = R""""(
+---+
|    
|    
+---+
|   |
|   |
+---+
)"""";
string seven = R""""(
+---+
    |
    |
    +
    |
    |
    +
)"""";
string eight = R""""(
+---+
|   |
|   |
+---+
|   |
|   |
+---+
)"""";
string nine = R""""(
+---+
|   |
|   |
+---+
    |
    |
+---+
)"""";
string separator = R""""(
 
 
o
 
o
 
 
)"""";

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
        code = split(code, "\n", 2)[1];
        code_arrays.pb(split(code, "\n"));
    }
    separator = split(separator, "\n", 2)[1];
    vs separator_array = split(separator, "\n");
    
    while(true) {
        string s; cin >> s;
        if(s == "end") {
            cout << s << endl;
            break;
        }
        vvs time = {code_arrays[s[0] - '0'], code_arrays[s[1] - '0'], 
            separator_array,
            code_arrays[s[3] - '0'], code_arrays[s[4] - '0']};
        vs output = join(time, "  ");
        print(output);
        cout << endl << endl;
    }
}
```