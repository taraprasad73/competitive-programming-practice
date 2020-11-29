## Reading till EOF
```cpp
string line;
while (getline(cin, line)) {
    // use the line
}
```

## Get lines ignoring first new line
Get lines ignoring everything till the first new line. Useful int the following scenario.
```
// input
B
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
vs get_lines(int rows) {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    vs input(rows);
    for(int i = 0; i < rows; ++i) {
        getline(cin, input[i]);
    }
    return input;
}
```