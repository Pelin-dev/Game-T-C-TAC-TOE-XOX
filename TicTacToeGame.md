#include <stdlib.h>
#include <stdio.h>

int control(char mat[][100], int i, int j) {
    if (mat[i][j] == 'X' || mat[i][j] == 'O') {
        return 0; // koordinat dolu
    } else {
        return 1; // koordinat boş
    }
}

// Hamle sonrası kazanma kontrolü: yatay, dikey ve çapraz üçlü eşleşme
int final(char mat1[][100], int i, int j, int n) {
    char symbol = mat1[i][j];

    // Yatay
    if (j >= 1 && j < n-1 &&
        mat1[i][j-1] == symbol &&
        mat1[i][j+1] == symbol) {
        return 1;
    }
    // Dikey
    if (i >= 1 && i < n-1 &&
        mat1[i-1][j] == symbol &&
        mat1[i+1][j] == symbol) {
        return 1;
    }
    // Sol üst – sağ alt çapraz
    if (i >= 1 && j >= 1 && i < n-1 && j < n-1 &&
        mat1[i-1][j-1] == symbol &&
        mat1[i+1][j+1] == symbol) {
        return 1;
    }
    // Sağ üst – sol alt çapraz
    if (i >= 1 && j >= 1 && i < n-1 && j < n-1 &&
        mat1[i-1][j+1] == symbol &&
        mat1[i+1][j-1] == symbol) {
        return 1;
    }

    return 0;
}

void printBoard(char mat[][100], int n) {
    printf("\n");
    for (int r = 0; r < n; r++) {
        for (int c = 0; c < n; c++) {
            printf(" %c ", mat[r][c]);
            if (c < n-1) printf("|");
        }
        printf("\n");
        if (r < n-1) {
            for (int k = 0; k < n; k++) {
                printf("---");
                if (k < n-1) printf("+");
            }
            printf("\n");
        }
    }
    printf("\n");
}

int main() {
    int n, i, j;
    char symbol;

    printf("Enter a size for playground (5–100): ");
    if (scanf("%d", &n) != 1 || n < 5 || n > 100) {
        printf("Invalid size. Please enter a number between 5 and 100.\n");
        return 1;
    }

    char matrix[100][100];
    for (i = 0; i < n; i++)
        for (j = 0; j < n; j++)
            matrix[i][j] = ' ';

    char player1, player2;
    printf("Which symbol will player1 use? (X/O): ");
    scanf(" %c", &symbol);
    if (symbol == 'X' || symbol == 'x') {
        player1 = 'X';  player2 = 'O';
    } else {
        player1 = 'O';  player2 = 'X';
        symbol = 'O';
    }
    printf("First player: %c, Second player: %c\n", player1, player2);

    int maxMoves = n * n;
    for (int move = 1; move <= maxMoves; move++) {
        printBoard(matrix, n);
        printf("Player %c, enter your move (row col): ", symbol);
        if (scanf("%d %d", &i, &j) != 2) {
            printf("Invalid input. Try again.\n");
            while (getchar() != '\n');
            move--;
            continue;
        }
        if (i < 0 || i >= n || j < 0 || j >= n) {
            printf("Coordinates out of range. Try again.\n");
            move--;
            continue;
        }
        if (!control(matrix, i, j)) {
            printf("Cell already occupied. Try again.\n");
            move--;
            continue;
        }

        matrix[i][j] = symbol;
        if (final(matrix, i, j, n)) {
            printBoard(matrix, n);
            printf("Player %c wins!\n", symbol);
            return 0;
        }
        // Sembol değiştir
        symbol = (symbol == 'X') ? 'O' : 'X';
    }

    printBoard(matrix, n);
    printf("It's a draw.\n");
    return 0;
}
