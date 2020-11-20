```cpp
typedef struct Square_ {
    bool attacked;
    bool occupied;
    char occupied_by;
} Square;

typedef struct ChessBoard_ {
    Square block[8][8];
    pair<int, int> white_king_pos;
    pair<int, int> black_king_pos;
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

void set_white_attack_field(ChessBoard *board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            switch (board->block[row][col].occupied_by) {
            case 'P':
                pawn_field(board, row, col);
                break;
            case 'K':
                king_field(board, row, col);
                break;
            case 'R':
                rook_field(board, row, col);
                break;
            case 'N':
                knight_field(board, row, col);
                break;
            case 'Q':
                queen_field(board, row, col);
                break;
            case 'B':
                bishop_field(board, row, col);
            }
        }
    }
}

void set_black_attack_field(ChessBoard *board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            switch (board->block[row][col].occupied_by) {
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

void input_board(ChessBoard *board) {
    char piece;
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            scanf("%c", &piece);
            board->block[row][col].occupied_by = piece;

            if (piece == '.')
                board->block[row][col].occupied = false;
            else
                board->block[row][col].occupied = true;
            board->block[row][col].attacked = false;

            if (piece == 'k')
                board->black_king_pos = make_pair(row, col);
            if (piece == 'K')
                board->white_king_pos = make_pair(row, col);
        }
        scanf("\n");
    }
}

bool board_is_empty(ChessBoard board) {
    for (int row = 0; row < 8; row++) {
        for (int col = 0; col < 8; col++) {
            if (board.block[row][col].occupied_by != '.') {
                return false;
            }
        }
    }
    return true;
}

void answer(ChessBoard board, int case_num) {
    ChessBoard board_copy = board;
    set_white_attack_field(&board_copy);
    if (board_copy.block[board.black_king_pos.first][board.black_king_pos.second].attacked) {
        printf("Game #%d: black king is in check.\n", case_num);
        return;
    }
    board_copy = board;
    set_black_attack_field(&board_copy);
    if (board_copy.block[board.white_king_pos.first][board.white_king_pos.second].attacked) {
        printf("Game #%d: white king is in check.\n", case_num);
        return;
    }
    printf("Game #%d: no king is in check.\n", case_num);
}

int main() {
    //freopen("input.txt", "r", stdin);
    //freopen("output.txt", "w", stdout);
    int case_num = 1;
    while (1) {
        ChessBoard board;
        input_board(&board);
        //print_board(board);
        if (board_is_empty(board))
            break;
        answer(board, case_num);
        case_num++;
    }
    return 0;
}
```