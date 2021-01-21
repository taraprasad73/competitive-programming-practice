## Reading till EOF
```cpp
string line;
while (getline(cin, line)) {
    istringstream line_stream(line);
    int Ar, Ac, Tr, Tc; line_stream >> Ar >> Ac >> Tr >> Tc;
}
```

## Get lines ignoring first new line
Get lines ignoring everything till the first new line (space also have to be captured). Useful in the following scenario.
```
// input
*. *. **
.. *. ..
.. .. ..
```
```cpp
// This wouldn't work.
char c; cin >> c;
vector<string> vs;
for(int i = 0; i < 3; ++i)
    getline(cin, vs[i]);
```
```cpp
// This works.
vs get_lines(int rows) {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    vs input(rows);
    for(int i = 0; i < rows; ++i) {
        getline(cin, input[i]);
    }
    return input;
}
```

## Input separated by empty lines
```
2

10111000
00010100
00111000
00111010
00111111
01011110
01011110
00011110

10111000
00010100
00111000
00111010
00111111
01011110
01011110
00011110
```
```cpp
int t; cin >> t;
string line; getline(cin, line); getline(cin, line);
while(t--) {
    vector<string> input;
    while(true) {
        string line; getline(cin, line);
        if(line.empty()) break;
        input.pb(line);
    }
    print(input);
}
```