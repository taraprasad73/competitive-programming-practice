```cpp
void out_shuffle(vi& cards) {
    int n = cards.size();
    vi half1(cards.begin(), cards.begin() + (n + 1)/2);
    vi half2(cards.begin() + (n + 1)/2, cards.end());
    for(int i = 0, j = 0; i < n; i += 2, ++j) {
        cards[i] = half1[j];
    }
    for(int i = 1, j = 0; i < n; i += 2, ++j) {
        cards[i] = half2[j];
    }
}

void in_shuffle(vi& cards) {
    int n = cards.size();
    vi half1(cards.begin(), cards.begin() + n/2);
    vi half2(cards.begin() + n/2, cards.end());
    for(int i = 0, j = 0; i < n; i += 2, ++j) {
        cards[i] = half2[j];
    }
    for(int i = 1, j = 0; i < n; i += 2, ++j) {
        cards[i] = half1[j];
    }
}

int main() {
    ios::sync_with_stdio(false); cin.tie(NULL); // Turn Off for Interactive Problems
#ifdef LOCAL_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    int n; cin >> n;
    string type; cin >> type;
    vi cards(n);
    for(int i = 0; i < n; ++i) {
        cards[i] = i;
    }
    vi temp = cards;
    int ans = 0;
    if(type == "in") {
        do {
            ++ans;
            in_shuffle(temp);
        } while(cards != temp);
    } else {
        do {
            ++ans;
            out_shuffle(temp);
        } while(cards != temp);
    }
    cout << ans;
    return 0;
}
```