## Whitespace
```cpp
void removeWhiteSpace(string &s) {
    s.erase(remove_if(ALL(s), [](char x) {
        return isspace(x);
    }), s.end());
}
```

## Split string with regex
```cpp
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

vector<string> split(string const& s) {
    regex regex{R"([\s,]+)"}; // split on space and comma
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}
```

## split string int n parts with regex
```cpp
template<typename T>
string join(vector<T> const& v, string delim="") {
    string s;
    bool first = true;
    for (auto const& piece: v) {
        if(first) first = false;
        else s += delim;
        s += piece;
    }
    return s;
}
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}
vector<string> split(string const& s, string delim, uint parts=0) {
    vector<string> tokens = split(s, regex{"([" + delim + "])"});
    if(parts != 0 and tokens.size() >= parts) {
        tokens[parts - 1] = join(vector<string>(tokens.begin() + parts - 1, tokens.end()), delim);
        tokens.erase(tokens.begin() + parts, tokens.end());
    }
    return tokens;
}
```

## Join vector to forma string
join vector to form a string, works for both string and char
```cpp
template<typename T>
string join(vector<T> const& v, string delim="") {
    string s;
    bool first = true;
    for (auto const& piece: v) {
        if(first) first = false;
        else s += delim;
        s += piece;
    }
    return s;
}
```

## split string array to units of string arrays 
```
split vector<string> into units of vector<vector<string>> by a column delimiter
{"*. *. ** **",     {{"*.", "..", ".."},
 ".. *. .. ..",  --> {"*.", "*.", ".."},
 ".. .. .. .."}      {"**", "..", ".."},
                     {"**", "..", ".."}}
```
```cpp
vector<vector<string>> split(vector<string> const& input, int unit_width, string delim="") {
    int total_width = unit_width + delim.length();
    // add extra delim length to make it perfectly divisible by total_width
    int num_units = (input[0].length() + delim.length()) / total_width; 
    int unit_height = input.size();
    vector<vector<string>> output(num_units);
    for(int k = 0; k < num_units; ++k) {
        vector<string> unit(unit_height, string(unit_width, ' '));
        for(int i = 0; i < unit_height; ++i) {
            for(int j = k * total_width; j < k * total_width + unit_width; ++j) {
                unit[i][j % total_width] = input[i][j];
            }
        }
        output[k] = unit;
    }
    return output;
}
```
## units of string arrays into one single string array
```
merges vector<string> units side by side, separated by a column delim to form vector<vector<string>>
{{"*.", "..", ".."},         {"*. *. ** **",     
 {"*.", "*.", ".."},   -->    ".. *. .. ..",
 {"**", "..", ".."},          ".. .. .. .."}      
 {"**", "..", ".."}}
```
```cpp
vector<string> join(vector<vector<string>> const& units, string delim="") {
    vector<string> output(units[0].size());
    for(int i = 0; i < units[0].size(); ++i) {
        for(int j = 0; j < units.size(); ++j) {
            if(j != 0) 
                output[i] += delim;
            output[i] += units[j][i];
        }
    }
    return output;
}
```