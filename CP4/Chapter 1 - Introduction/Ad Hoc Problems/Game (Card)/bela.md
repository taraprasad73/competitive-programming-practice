```cpp
int value(char dominant, char value, char suit) {
    switch(value) {
        case 'A':
        return 11;
        case 'K':
        return 4;
        case 'Q':
        return 3;
        case 'J':
        if(suit == dominant) {
            return 20;
        } else {
            return 2;
        }
        case 'T':
        return 10;
        case '9':
        if(suit == dominant) {
            return 14;
        } else {
            return 0;
        }
        case '8':
        case '7':
        return 0;
    }
    return 0;
}

int main() {
    ios::sync_with_stdio(false); cin.tie(NULL); // Turn Off for Interactive Problems
#ifdef LOCAL_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
    int N; char B; cin >> N >> B;
    int ans = 0;
    for(int i = 0; i < 4 * N; ++i) {
        string s; cin >> s;
        ans += value(B, s[0], s[1]);
        wi(ans)
    }
    cout << ans << endl;
    return 0;
}
```