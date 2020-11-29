```cpp
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

// G:                                                           
// F: ----------------------------------------------------------
// E:                                                           
// D: ----------------------------------------------------------
// C:                                                           
// B: ----------------------------------------------------------
// A:                                                           
// g: ----------------------------------------------------------
// f:                                                           
// e: ----------------------------------------------------------
// d:                                                           
// c:                                                           
// b:                                                           
// a: ----------------------------------------------------------
vs convert(map<char, string> staff) {
    vs output;
    output.pb(staff['G']);
    output.pb(staff['F']);
    output.pb(staff['E']);
    output.pb(staff['D']);
    output.pb(staff['C']);
    output.pb(staff['B']);
    output.pb(staff['A']);
    output.pb(staff['g']);
    output.pb(staff['f']);
    output.pb(staff['e']);
    output.pb(staff['d']);
    output.pb(staff['c']);
    output.pb(staff['b']);
    output.pb(staff['a']);
    return output;
}

map<char, string> get_staff() {
    map<char, string> staff;
    staff['G'] = " ";
    staff['F'] = "-";
    staff['E'] = " ";
    staff['D'] = "-";
    staff['C'] = " ";
    staff['B'] = "-";
    staff['A'] = " ";
    staff['g'] = "-";
    staff['f'] = " ";
    staff['e'] = "-";
    staff['d'] = " ";
    staff['c'] = " ";
    staff['b'] = " ";
    staff['a'] = "-";
    return staff;
}

void print(vs const& lines) {
    for(int i = 0; i < lines.size(); ++i) {
        cout << lines[i] << endl;
    }
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

string start = R""""(
G:
F:
E:
D:
C:
B:
A:
g:
f:
e:
d:
c:
b:
a:
)"""";

void solve() {    
    int n; cin >> n;
    vvs notes;
    for(int i = 0; i < n; ++i) {
        if(i != 0) 
            notes.pb(convert(get_staff()));
        string s; cin >> s;
        if(s.length() == 1) {
            auto staff = get_staff();
            staff[s[0]] = "*";
            notes.pb(convert(staff));
        } else {
            char note; int length;
            sscanf(s.c_str(), "%c%d", &note, &length);
            auto staff = get_staff();
            staff[s[0]] = "*";
            vvs rep_notes(length, convert(staff));
            notes.pb(join(rep_notes));
        }
    }
    vs merged_notes = join(notes);
    start = split(start, "\n", 2)[1];
    vvs output = {split(start, "\n"), merged_notes};
    print(join(output, " "));
}
```