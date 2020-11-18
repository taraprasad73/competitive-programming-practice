```cpp
struct card {
    string x;
    card(string _x) : x(_x) {}
    int value() {
        return convert(x[0]);
    }
private:
    int convert(char c) {
        if(isdigit(c)) {
            return int(c - '0');
        } else {
            return 10;
        }
    }
};

int main() {
    ios::sync_with_stdio(false); cin.tie(NULL); // Turn Off for Interactive Problems
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    int t; cin >> t;
    for(int k = 1; k <= t; ++k) {
        stack<card> pile;
        for(int i = 0; i < 52; ++i) {
            string x; cin >> x;
            pile.push(x);
        }
        int Y = 0;
        stack<card> top25; 
        for(int i = 0; i < 25; ++i) {
            top25.push(pile.top());
            pile.pop();
        }
        for(int i = 0; i < 3; ++i) {
            int v = pile.top().value();
            Y += v;
            pile.pop();
            for(int j = 0; j < 10 - v; ++j) {
                pile.pop();
            }
        }
        // now need to find the Yth card from bottom, after adding all the cards from top25 to pile
        while(not pile.empty()) {
            top25.push(pile.top());
            pile.pop();
        }
        for(int i = 0; i < Y - 1; ++i) {
            top25.pop();
        }
        cout << "Case " << k << ": " << top25.top().x << endl;
    }
    return 0;
}
```