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

void solve() {
    vi a;
    string line;
    while (getline(cin, line)) {
        istringstream line_stream(line);
        ll x; line_stream >> x;
        a.pb(x);
    }
    LIS<ll> lis(a);
    cout << lis.getLength() << endl;
    cout << "-" << endl;
    for(auto v: lis.lis_path_values) {
        cout << v << endl;
    }
}
```