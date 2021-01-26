# O(n^2) - just value
```cpp
/**
 * lis(n) = length of longest subsequence ending at position n
 * a: 0-indexed
 * memo: same size as a, initialized to -1
 */  
ll lis(int n, vi &a, vi &memo) {
    if(n == 0) return 1;
    if(memo[n] != -1) return memo[n];
    memo[n] = 1;
    for(int i = 0; i < n; ++i) {
        if(a[i] < a[n]) 
            memo[n] = max(memo[n], lis(i, a, memo) + 1);
    }
    return memo[n];
}
```

# O(n*log(k)) - just value
```cpp
/**
 * a: 0-indexed
 */  
template<typename T> 
int lis(vector<T> const& a) {
    vector<T> lis;
    for(int i = 0; i < SZ(a); ++i) {
        auto itr = lower_bound(lis.begin(), lis.end(), a[i]);
        if(itr == lis.end()) 
            lis.pb(a[i]);
        else 
            *itr = a[i];
    }
    return lis.size();
}
```

# O(n^2) with path
```cpp
/**
 * lis(n) = length of longest subsequence ending at position n
 * a: 0-indexed
 * memo: same size as a, initialized to -1
 */  
int lis_recursion(int n, vi &a, vi &memo, vi &parent) {
    if(n == 0) return 1;
    if(memo[n] != -1) return memo[n];
    memo[n] = 1;
    for(int i = 0; i < n; ++i) {
        if(a[i] < a[n]) {
            ll x = lis_recursion(i, a, memo, parent);
            if(x + 1 > memo[n]) { // need to change sometimes to >=
                memo[n] = x + 1;
                parent[n] = i;
            }
        }
    }
    return memo[n];
}

void extractPathIndices(int pos, vi const& parent, vi &path) {
    path.pb(pos);
    if(parent[pos] == -1) return;
    extractPathIndices(parent[pos], parent, path);
}

vi constructLis(int lis_pos, vi const& a, vi const& parent) {
    vi path;
    extractPathIndices(lis_pos, parent, path);
    reverse(ALL(path));
    vi lis(path.size());
    for(int i = 0; i < SZ(path); ++i) {
        lis[i] = a[path[i]];
    }
    return lis;
}

/**
 * a: 0-indexed
 */ 
vi computeLisPath(vi &a) {
    int max_lis = 0, lis_pos;
    int n = SZ(a);
    vi memo(n, -1), parent(n, -1);
    for(int i = 0; i < SZ(a); ++i) {
        int x = lis_recursion(i, a, memo, parent);
        if(x > max_lis) { // need to change sometimes to >=
            max_lis = x;
            lis_pos = i; 
        }
    }
    return constructLis(lis_pos, a, parent);
}
```

# O(n^2) - OOP with path
## Class
```cpp
template<typename T>
class LIS {
public:
    /**
     * a: 0-indexed
     */ 
    LIS(vi const& a) {
        int n = SZ(a);
        nodes.resize(n);
        for(int i = 0; i < n; ++i) {
            nodes[i].value = a[i];
        }
        compute();
    }

    vector<T> lis_path_values;
    vector<T> lis_path_positions;

    int getLength() {
        return lis_path_values.size();
    }

private:
    struct Node {
        T value;
        int parent = -1;
        int memo = -1; // dp
    };
    
    vector<Node> nodes;
    
    /**
     * lis(n) = length of longest subsequence ending at position n
     */  
    int lis_recursion(int n) {
        if(n == 0) return 1;
        if(nodes[n].memo != -1) return nodes[n].memo;
        nodes[n].memo = 1;
        for(int i = 0; i < n; ++i) {
            if(nodes[i].value < nodes[n].value) {
                ll x = lis_recursion(i);
                if(x + 1 > nodes[n].memo) {
                    nodes[n].memo = x + 1;
                    nodes[n].parent = i;
                }
            }
        }
        return nodes[n].memo;
    }

    void constructLisPath(int pos) {
        if(nodes[pos].parent == -1) {
            lis_path_positions.pb(pos);
            lis_path_values.pb(nodes[pos].value);
            return;
        }
        constructLisPath(nodes[pos].parent);
        lis_path_positions.pb(pos);
        lis_path_values.pb(nodes[pos].value);
    }

    void compute() {
        int max_lis = 0, lis_pos;
        for(int i = 0; i < SZ(nodes); ++i) {
            int x = lis_recursion(i);
            if(x > max_lis) { // need to change sometimes to >=
                max_lis = x;
                lis_pos = i; 
            }
        }
        constructLisPath(lis_pos);
    }
};
```
## Usage
```cpp
void solve() {
    int n; cin >> n;
    vi a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    LIS<ll> lis(a);
    wi(lis.getLength(), lis.lis_path_positions, lis.lis_path_values)
}
```

# O(n*log(n)) Greedy + D&C with path
## Class
```cpp
template<typename T>
class LIS {
public:
    LIS(vi const& a) {
        int n = SZ(a);
        nodes.resize(n);
        for(int i = 0; i < n; ++i) {
            nodes[i].value = a[i];
        }
        compute();
    }

    vector<T> lis_path_values;
    vector<T> lis_path_positions;

    int getLength() {
        return lis_path_values.size();
    }

private:
    struct Node {
        T value;
        int parent = -1;
    };
    vector<Node> nodes;
    vector<T> lis; // smallest ending value of all (i+1) length LIS found so far
    vi lis_pos; // position of smallest ending value of all (i+1) length LIS found so far

    void constructLisPath(int pos) {
        if(nodes[pos].parent == -1) {
            lis_path_positions.pb(pos);
            lis_path_values.pb(nodes[pos].value);
            return;
        }
        constructLisPath(nodes[pos].parent);
        lis_path_positions.pb(pos);
        lis_path_values.pb(nodes[pos].value);
    }

    /**
     * Computes the last longest increasing subsequence
     * [1 3 5 2 5] -> [1 -> 2 -> 5] not [1 -> 3 -> 5]
     */ 
    void compute() {
        for(int i = 0; i < SZ(nodes); ++i) {
            auto itr = lower_bound(lis.begin(), lis.end(), nodes[i].value);
            int pos = itr - lis.begin();
            if(itr == lis.end()) {
                lis.pb(nodes[i].value);
                lis_pos.pb(i);
            } else {
                *itr = nodes[i].value;
                lis_pos[pos] = i;
            }
            nodes[i].parent = pos == 0 ? -1 : lis_pos[pos - 1];
        }
        constructLisPath(*prev(lis_pos.end()));
    }
};
```
## Usage
```cpp
void solve() {
    int n; cin >> n;
    vi a(n);
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    LIS<ll> lis(a);
    wi(lis.getLength(), lis.lis_path_positions, lis.lis_path_values)
}
```