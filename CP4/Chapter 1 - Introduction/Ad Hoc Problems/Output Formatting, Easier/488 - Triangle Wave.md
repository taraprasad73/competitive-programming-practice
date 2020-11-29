```cpp
void print_waveform(int a) {
    for(int i = 1; i < a; ++i) {
        cout << string(i, '0' + i) << endl;
    }
    for(int i = a; i >= 1; --i) {
        cout << string(i, '0' + i) << endl;        
    }
}

void solve() {
    int t; cin >> t;
    bool first = true;
    while(t--) {
        if(first) {
           first = false; 
        } else {
            cout << endl;
        }
        int amp, freq; cin >> amp >> freq;
        for(int i = 0; i < freq; ++i) {
            if(i != 0) cout << endl;
            print_waveform(amp);
        }
    }
}
```