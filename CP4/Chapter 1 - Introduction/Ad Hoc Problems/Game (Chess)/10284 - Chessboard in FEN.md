```cpp
typedef struct Square_ {
    bool attacked;
    bool occupied;
    char occupied_by;
} Square;

typedef struct ChessBoard_ {
    Square block[8][8];
} ChessBoard;

void print_board(ChessBoard board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            if (board.block[row][col].occupied)
                printf("%c", board.block[row][col].occupied_by);
            else if (board.block[row][col].attacked)
                printf("*");
            else {
                printf(".");
            }
        }
        printf("\n");
    }
    printf("\n");
}

bool is_valid_index(int n) {
    if (n >= 0 && n < 8)
        return true;
    else
        return false;
}

void pawn_field(ChessBoard *board, int row, int col) {
    if (board->block[row][col].occupied_by == 'P') {
        if (row - 1 >= 0) {
            if (col - 1 >= 0)
                board->block[row - 1][col - 1].attacked = true;
            if (col + 1 < 8)
                board->block[row - 1][col + 1].attacked = true;
        }
    } else {
        if (row + 1 < 8) {
            if (col - 1 >= 0)
                board->block[row + 1][col - 1].attacked = true;
            if (col + 1 < 8)
                board->block[row + 1][col + 1].attacked = true;
        }
    }
}

void king_field(ChessBoard *board, int row, int col) {
    for (int i = row - 1; i <= row + 1; i++) {
        for (int j = col - 1; j <= col + 1; j++) {
            if (is_valid_index(i) && is_valid_index(j)
                    && !(i == row && j == col)) {
                board->block[i][j].attacked = true;
            }
        }
    }
}

void rook_field(ChessBoard *board, int row, int col) {
    for (int i = row + 1; i < 8; i++) {
        if (is_valid_index(i)) {
            board->block[i][col].attacked = true;
        }
        if (board->block[i][col].occupied)
            break;
    }

    for (int i = row - 1; i >= 0; i--) {
        if (is_valid_index(i)) {
            board->block[i][col].attacked = true;
        }
        if (board->block[i][col].occupied)
            break;
    }

    for (int i = col + 1; i < 8; i++) {
        if (is_valid_index(i)) {
            board->block[row][i].attacked = true;
        }
        if (board->block[row][i].occupied)
            break;
    }

    for (int i = col - 1; i >= 0; i--) {
        if (is_valid_index(i)) {
            board->block[row][i].attacked = true;
        }
        if (board->block[row][i].occupied)
            break;
    }
}

void knight_field(ChessBoard *board, int row, int col) {
    for (int i = row - 2; i <= row + 2; i++) {
        for (int j = col - 2; j <= col + 2; j++) {
            if (is_valid_index(i) && is_valid_index(j)) {
                bool cond1 = (i == row - 1 && (j == col - 2 || j == col + 2));
                bool cond2 = (i == row + 1 && (j == col - 2 || j == col + 2));
                bool cond3 = (i == row - 2 && (j == col - 1 || j == col + 1));
                bool cond4 = (i == row + 2 && (j == col - 1 || j == col + 1));
                if (cond1 || cond2 || cond3 || cond4)
                    board->block[i][j].attacked = true;
            }
        }
    }
}

void bishop_field(ChessBoard *board, int row, int col) {
    for (int i = row + 1, j = col + 1;; i++, j++) {
        if (is_valid_index(i) && is_valid_index(j)) {
            board->block[i][j].attacked = true;
        } else {
            break;
        }
        if (board->block[i][j].occupied)
            break;
    }

    for (int i = row + 1, j = col - 1;; i++, j--) {
        if (is_valid_index(i) && is_valid_index(j)) {
            board->block[i][j].attacked = true;
        } else {
            break;
        }
        if (board->block[i][j].occupied)
            break;
    }

    for (int i = row - 1, j = col + 1;; i--, j++) {
        if (is_valid_index(i) && is_valid_index(j)) {
            board->block[i][j].attacked = true;
        } else {
            break;
        }
        if (board->block[i][j].occupied)
            break;
    }
    for (int i = row - 1, j = col - 1;; i--, j--) {
        if (is_valid_index(i) && is_valid_index(j)) {
            board->block[i][j].attacked = true;
        } else {
            break;
        }
        if (board->block[i][j].occupied)
            break;
    }
}

void queen_field(ChessBoard *board, int row, int col) {
    bishop_field(board, row, col);
    rook_field(board, row, col);
}

void set_attack_field(ChessBoard *board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            switch (tolower(board->block[row][col].occupied_by)) {
            case 'p':
                pawn_field(board, row, col);
                break;
            case 'k':
                king_field(board, row, col);
                break;
            case 'r':
                rook_field(board, row, col);
                break;
            case 'n':
                knight_field(board, row, col);
                break;
            case 'q':
                queen_field(board, row, col);
                break;
            case 'b':
                bishop_field(board, row, col);
            }
        }
    }
}

void populate_board(ChessBoard *board, char *fen) {
    int row = 0;
    int col = 0;
    while (*fen != '\0') {
        char piece = *fen;
        if (piece == '/') {
            row++;
            col = 0;
        } else if (piece >= '1' && piece <= '8') {
            for (int i = 0; i < (piece - '0'); i++) {
                board->block[row][col++].occupied_by = ' ';
            }
        } else {
            board->block[row][col].occupied = true;
            board->block[row][col++].occupied_by = piece;
        }
        fen++;
    }
}

void initialize_board(ChessBoard *board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            board->block[row][col].occupied_by = ' ';
            board->block[row][col].attacked = false;
            board->block[row][col].occupied = false;
        }
    }
}

void answer(ChessBoard *board) {
    int ans = 0;
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            if (!board->block[row][col].occupied && !board->block[row][col].attacked) {
                ans++;
            }
        }
    }
    printf("%d\n", ans);
}

int main() {
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
    while (1) {
        char fen[100];
        if (scanf("%s\n", fen) == EOF)
            break;
        ChessBoard board;
        initialize_board(&board);
        populate_board(&board, fen);
        set_attack_field(&board);
        answer(&board);
        //print_board(board);
    }
    return 0;
}
```