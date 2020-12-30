```cpp
void printQueue(deque<int> const& s) {
    deque<int> q = s;
    cout << "Q: ";
    while(not q.empty()) {
        cout << q.front() << " "; q.pop_front();
    }
    cout << endl;
}

vvi answers(15);

void generate() {
    for(int n = 1; n <= 13; ++n) {
        vi ans(n);
        deque<int> q;
        for(int i = 0; i < n; ++i) {
            q.push_back(i);
        }
        for(int i = 1; i <= n; ++i) {
            for(int j = 0; j < i; ++j) {
                int x = q.front(); q.pop_front(); q.push_back(x);
            }
            int x = q.front();
            q.pop_front();
            ans[x] = i;
            answers[n] = ans;
        }
    }
}

void print(vi const& v) {
    for(auto x: v) {
        cout << x << " ";
    }
    cout << endl;
}

void solve() {
    generate();
    int t; cin >> t; 
    while(t--) {
        int n; cin >> n;
        print(answers[n]);
    }    
}
```