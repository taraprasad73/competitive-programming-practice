# [A. Suffix Array - 1](https://codeforces.com/edu/course/2/lesson/2/1/practice/contest/269100/problem/A)
```cpp
vector<pair<char, int> > base_case_preprocessing(string const& s) {
    int n = s.size();
    vector<pair<char, int> > a(n);
    for(int i = 0; i < n; ++i) {
        a[i] = {s[i], i};
    }
    sort(ALL(a));
    return a;
}
 
// c -> equivalence classes of k - 1 iteration
vector<pair<ii, int> > normal_case_preprocessing(int k, string const& s, vi& c) {
    int n = s.size();
    vector<pair<ii, int> > a(n); /* Each string is going to be represented by a pair of numbers, instead of just a character.
                              This pair corresponds to the equivalence class of its left half and right half. */
    for(int i = 0; i < n; ++i) {
        a[i] = {{c[i], c[(i + TWO_POWER(k - 1)) % n]}, i};
    }
    sort(ALL(a));
    return a;
}
 
/* c -> equivalence classes (like rank), for a given iteration k, c[i] = rank of substring 
            starting at position i of s, having length 2^k */
template <typename T> 
vi get_equivalence_classes(vector<pair<T, int>> const& a) {
    int n = a.size();
    vi c(n);
    c[a[0].S] = 0;
    for(int i = 1; i < n; ++i) {
        if(a[i].F == a[i - 1].F) {
            c[a[i].S] = c[a[i - 1].S];
        } else {
            c[a[i].S] = c[a[i - 1].S] + 1;
        }
    }
    return c;
}
 
vi get_suffix_array(string const& s) {
    int n = s.size(); 
    
    // base case
    int k = 0;
    vector<pair<char, int>> a_0 = base_case_preprocessing(s);
    vi c = get_equivalence_classes(a_0);
    
    // main loop
    vector<pair<ii, int>> a_k;
    do {
        ++k;
        a_k = normal_case_preprocessing(k, s, c);
        c = get_equivalence_classes(a_k);    
    } while(TWO_POWER(k) < n);
    
    vi p(n);
    for(int i = 0; i < n; ++i) {
        p[i] = a_k[i].S;
    }
    return p;
}
```