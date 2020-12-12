```cpp
map<string, int> order = {{"O-", 0}, {"O+", 1}, {"AB-", 2}, {"AB+", 3}, 
    {"B-", 4}, {"B+", 5}, {"A-", 6}, {"A+", 7}};

bool cmp(string a, string b) {
    return order[a] < order[b];
}

void print(vector<string> s) {
    sort(ALL(s), cmp);
    cout << "{";
    for(auto it = s.begin(); it != s.end(); it++) {
        cout << *it;
        if(it != prev(s.end())) {
            cout << ", ";
        }
    }
    cout << "}";
}

map<string, vector<char>> combinations;
map<set<char>, string> types;
    
vector<string> getChildTypes(string a, string b) {
    char allele1 = *prev(a.end()), allele2 = *prev(b.end()); 
    a = string(a.begin(), prev(a.end()));
    b = string(b.begin(), prev(b.end()));

    vector<char> alleles;
    if(allele1 == '-' && allele2 == '-') {
        alleles.pb('-');
    } else {
        alleles.pb('-');
        alleles.pb('+');
    }
    set<string> blood_types;
    for(char a1: combinations[a]) {
        for(char a2: combinations[b]) {
            set<char> pool = {a1, a2};
            blood_types.insert(types[pool]);
        }
    }
    vector<string> answer;
    for(string blood_type: blood_types) {
        for(char allele: alleles) {
            answer.pb(blood_type + allele);
        }
    }
    return answer;
}

void solve() {
    combinations["O"] = {'O'};
    combinations["B"] = {'B', 'O'};
    combinations["A"] = {'A', 'O'};
    combinations["AB"] = {'A', 'B'};

    types[{'O'}] = "O";
    types[{'A', 'O'}] = "A";
    types[{'A'}] = "A";
    types[{'B', 'B'}] = "B";
    types[{'B', 'O'}] = "B";
    types[{'A', 'B'}] = "AB";

    for(int t = 1; ; ++t) {
        string a, b, c; cin >> a >> b >> c;
        if(a == "E") break;
        cout << "Case " << t << ": ";
        if(c == "?") {
            vector<string> childTypes = getChildTypes(a, b);            
            cout << a << " " << b << " ";
            if(childTypes.size() == 1) 
                cout << childTypes[0];
            else 
                print(childTypes);
        } else {
            vector<string> parentTypes;
            for (auto [key, _] : combinations) {
                for(char x: {'-', '+'}) {
                    string bloodType = key + x;
                    string parentType = a == "?" ? b : a;
                    vector<string> childTypesVector = getChildTypes(parentType, bloodType);
                    set<string> childTypes = set<string>(ALL(childTypesVector));
                    if(CONTAINS(childTypes, c)) {
                        parentTypes.pb(bloodType);
                    }
                }
            }
            if(a == "?") {
                if(parentTypes.empty()) 
                    cout << "IMPOSSIBLE";
                else if(parentTypes.size() == 1) 
                    cout << parentTypes[0];
                else
                    print(parentTypes);
                cout << " " << b << " " << c;
            } else {
                cout << a << " ";
                if(parentTypes.empty()) 
                    cout << "IMPOSSIBLE";
                else if(parentTypes.size() == 1) 
                    cout << parentTypes[0];
                else
                    print(parentTypes);
                cout << " " << c;
            }
        }
        cout << endl;
    }
}
```