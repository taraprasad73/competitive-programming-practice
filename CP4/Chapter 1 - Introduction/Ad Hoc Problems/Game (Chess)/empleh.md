```cpp
vector<string> split(string const& s) {
    regex regex{R"([\s,]+)"}; // split on space and comma
    sregex_token_iterator it{s.begin(), s.end(), regex, -1};
    return {it, {}};
}

void populateWhite(vector<vector<char>> &board, string x) {
    int row, col;
    if(x.length() == 2) {
        row = 8 - (x[1] - '0');
        col = x[0] - 'a';
        board[row][col] = 'P';
    } else if(x.length() == 3) {
        row = 8 - (x[2] - '0');
        col = x[1] - 'a';
        board[row][col] = x[0];
    }
}

void populateBlack(vector<vector<char>> &board, string x) {
    int row, col;
    if(x.length() == 2) {
        row = 8 - (x[1] - '0');
        col = x[0] - 'a';
        board[row][col] = 'p';
    } else if(x.length() == 3) {
        row = 8 - (x[2] - '0');
        col = x[1] - 'a';
        board[row][col] = tolower(x[0]);
    }
}

void print(vector<vector<char>> board) {
    string border = "+---+---+---+---+---+---+---+---+";
    cout << border << endl;
    for(int i = 0; i < 8; ++i) {
        cout << "|";
        for(int j = 0; j < 8; ++j) {
            if((i + j) % 2 == 1) {
                if(board[i][j] == '*') {
                    cout << ":::";
                } else {
                    cout << ":" << board[i][j] << ":";
                }
            } else {
                if(board[i][j] == '*') {
                    cout << "...";
                } else {
                    cout << "." << board[i][j] << ".";
                }
            }
            cout << "|";
        }
        cout << endl << border << endl;
    }
}

void solve() {
    string input; 
    getline(cin, input);
    vector<string> whites = split(input.substr(7));
    getline(cin, input);
    vector<string> blacks = split(input.substr(7));
    
    vector<vector<char>> board(8, vector<char>(8, '*'));
    for(string x: whites)
        populateWhite(board, x);
    for(string x: blacks)
        populateBlack(board, x);
    
    print(board);
}
```