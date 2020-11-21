```cpp
struct expr {
    string a;
    string b;
    string op;
    expr() {}
    expr(string _a, string _op, string _b) : op(_op), a(_a), b(_b) {}
    friend ostream& operator<<(ostream& os, const expr& c);
};

ostream& operator<<(ostream& os, const expr& c) {
    os << "[" << c.a << " " << c.op << " " << c.b << "]";
    return os;
}

void solve() {
    int t; cin >> t; 
    for(int x = 1; x <= t; ++x) {
        int n; cin >> n;
        map<string, expr> variables;
        map<string, string> final_expression;
        string last_variable;
        for(int i = 0; i < n; ++i) {
            string left, first, second, operation, equals;
            cin >> left >> equals >> first >> operation >> second;
            variables[left] = expr(first, operation, second);
            string expression;
            if(CONTAINS(variables, first)) {
                // requires parenthesis for case: (2 + 3) * ...
                if(variables[first].op == "+" and operation == "*") {
                    expression += "(" + final_expression[first] + ")";
                } else {
                    expression += final_expression[first];
                }
            } else {
                expression += first;
            }
            expression += operation;
            // 4 cases: first + ( + ), first + ( * ), first * ( + ), first * ( * )
            // second case doesn't require parenthesis
            if(CONTAINS(variables, second)) {
                if(variables[second].op == "*" and operation == "+") {
                    expression += final_expression[second];
                } else {
                    expression += "(" + final_expression[second] + ")";
                }
            } else {
                expression += second;
            }
            final_expression[left] = expression;
            last_variable = left;
        }
        cout << "Expression #" << x << ": " << final_expression[last_variable] << endl;
    }
}
```