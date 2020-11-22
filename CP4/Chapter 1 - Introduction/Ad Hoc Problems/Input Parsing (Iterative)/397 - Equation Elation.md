Note: Although not mentioned in the problem statement, unary + needs to be discarded to get AC (as shown in sample output). Spaces are allowed in solution as mentioned though.
```cpp
vector<string> split(string const& s, regex regex) {
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

bool isOperator(string s) {
    return s == "+" or s == "*" or s == "/" or s == "-";
}

bool isOperator(char c) {
    return c == '*' or c == '/' or c == '+' or c == '-';
}

string addSpaces(string lhs) {
    string spaced_eq;
    for(char c: lhs) {
        if(isOperator(c)) {
            spaced_eq += " ";
            spaced_eq += c;
            spaced_eq += " "; 
        } else {
            spaced_eq += c;
        }
    }
    return spaced_eq;
}

vector<string> mergeUnary(vector<string> & tokens) {
    string prevToken = "";
    vector<string> result;
    for(int i = 0; i < tokens.size(); ++i) {
        if(isOperator(tokens[i]) and isOperator(prevToken)) {
            // current token is unary operator
            // unary + needs to be discarded to get AC
            if(tokens[i] == "-")
                tokens[i + 1] = tokens[i] + tokens[i + 1];
        } else {
            result.pb(tokens[i]);
        }
        prevToken = tokens[i];
    }
    return result;
}

void removeWhiteSpace(string &s) {
    s.erase(remove_if(ALL(s), [](char x) {
        return isspace(x);
    }), s.end());
}

void printEquation(vector<string> const& tokens, string rhs) {
    // the spaces are optional according to problem statement
    for(string t: tokens) 
        cout << t << " ";
    removeWhiteSpace(rhs);
    cout << "= " << rhs << endl;
}

int operate(string operand1, string operand2, string symbol) {
    int a = stoi(operand1);
    int b = stoi(operand2);
    if(symbol == "+")
        return a + b;
    if(symbol == "-")
        return a - b;
    if(symbol == "/")
        return a / b;
    if(symbol == "*")
        return a * b;
    return 0;
}

void executeOperation(vector<string> &tokens, int i) {
    tokens[i] = to_string(operate(tokens[i - 1], tokens[i + 1], tokens[i]));
    // CAUTION: i + 1 needs to be delete first
    tokens.erase(tokens.begin() + i + 1);
    tokens.erase(tokens.begin() + i - 1);
}

void solve() {
    string equation;
    bool first = true;
    while (getline(cin, equation)) {
        if(not first) cout << endl;
        else first = false;
        vector<string> parts = split(equation, regex{R"([=])"});
        string spaced_eq = addSpaces(parts[0]);
        vector<string> tokens = split(spaced_eq, regex{R"([\s]+)"});
        tokens = mergeUnary(tokens);
        printEquation(tokens, parts[1]);
        while(tokens.size() > 1) {
            bool done = false;
            for(int i = 0; i < tokens.size(); ++i) {
                if(tokens[i] == "*" or tokens[i] == "/") {
                    executeOperation(tokens, i);
                    done = true;
                    break;
                }
            }
            if(not done) {
                for(int i = 0; i < tokens.size(); ++i) {
                    if(tokens[i] == "+" or tokens[i] == "-") {
                        executeOperation(tokens, i);
                        break;
                    }
                }
            }
            printEquation(tokens, parts[1]);
        }
    }
}
```