```cpp
struct cell_t {
    bool occupied = false;
    bool reachableByQueen = false;
    bool reachableByKing = false;
    friend ostream& operator<<(ostream& os, const cell_t& c);
};
ostream& operator<<(ostream& os, const cell_t& c) {
    os << "{" << c.occupied << "," << c.reachableByKing << "," << c.reachableByQueen << "}";
    return os;
}
typedef vector<vector<cell_t>> board_t;

void placeQueen(board_t &b, int pos) {
    int x = (pos / 8) + 1;
    int y = (pos % 8) + 1;
    b[x][y].occupied = true;
    b[x][y].reachableByKing = false;
    for(int i = x + 1; i <= 8; ++i) {
        if(b[i][y].occupied)
            break;
        b[i][y].reachableByQueen = true;
    }
    for(int i = x - 1; i >= 1; --i) {
        if(b[i][y].occupied)
            break;
        b[i][y].reachableByQueen = true;
    }
    for(int i = y + 1; i <= 8; ++i) {
        if(b[x][i].occupied)
            break;
        b[x][i].reachableByQueen = true;
    }
    for(int i = y - 1; i >= 1; --i) {
        if(b[x][i].occupied)
            break;
        b[x][i].reachableByQueen = true;
    }
}

void placeKing(board_t &b, int pos) {
    int x = (pos / 8) + 1;
    int y = (pos % 8) + 1;
    b[x][y].occupied = true;
    if(x + 1 <= 8) b[x + 1][y].reachableByKing = true;
    if(y + 1 <= 8) b[x][y + 1].reachableByKing = true;
    if(x - 1 >= 1) b[x - 1][y].reachableByKing = true;
    if(y - 1 >= 1) b[x][y - 1].reachableByKing = true;
}

bool isMoveLegalForQueen(board_t& b, int pos) {
    int x = (pos / 8) + 1;
    int y = (pos % 8) + 1;
    return b[x][y].reachableByQueen;
}

bool isMoveAllowedForQueen(board_t& b, int pos) {
    int x = (pos / 8) + 1;
    int y = (pos % 8) + 1;
    return b[x][y].reachableByQueen and not b[x][y].reachableByKing;
}

bool isMoveAllowedForKing(board_t& b, int pos) {
    int x = (pos / 8) + 1;
    int y = (pos % 8) + 1;
    return (b[x + 1][y].reachableByKing and not b[x + 1][y].reachableByQueen)
        or (b[x][y + 1].reachableByKing and not b[x][y + 1].reachableByQueen)
        or (b[x - 1][y].reachableByKing and not b[x - 1][y].reachableByQueen)
        or (b[x][y - 1].reachableByKing and not b[x][y - 1].reachableByQueen);
}

void solve() {
    string line;
    while (getline(cin, line)) {
        int kpos, qpos, qmove;
        sscanf(line.c_str(), "%d %d %d", &kpos, &qpos, &qmove);
        board_t board(10, vector<cell_t>(10)); 
        placeKing(board, kpos);
        placeQueen(board, qpos);
        if(kpos == qpos) {
            cout << "Illegal state";
        } else if(not isMoveLegalForQueen(board, qmove)) {
            cout << "Illegal move";
        } else if(not isMoveAllowedForQueen(board, qmove)) {
            cout << "Move not allowed";
        } else {
            board = board_t(10, vector<cell_t>(10));
            placeKing(board, kpos);
            placeQueen(board, qmove);
            if(isMoveAllowedForKing(board, kpos)) {
                cout << "Continue";
            } else {
                cout << "Stop";
            }
        }
        cout << endl;
    }
}
```