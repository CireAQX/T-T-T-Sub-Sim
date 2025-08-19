#include <iostream>
#include <fstream>
using namespace std;

//////////////////////////////
//Board Creation 

void playBoardVisual(char board[3][3]) {

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {

            cout << board[i][j] << " ";
        }
        cout << endl;
    }
    cout << endl;
}

//////////////////////////////
// Establishing "Winner" Rules

char declareWinner(char board[3][3]) {
    for (int i = 0; i < 3; i++) {

        if (board[i][0] == board[i][1] && board[i][1] == board[i][2])
        {
            return board[i][0];
        }

        if (board[0][i] == board[1][i] && board[1][i] == board[2][i])
        {
            return board[0][i];
        }

    }

    if (board[0][0] == board[1][1] && board[1][1] == board[2][2])
    {
        return board[0][0];
    }

    if (board[0][2] == board[1][1] && board[1][1] == board[2][0])
    {
        return board[0][2];
    }

    return 'N';
}

//////////////////////////////
// Logic Variables

int main() {
    char board[3][3] = { {'1', '2', '3'}, {'4', '5', '6'}, {'7', '8', '9'} };
    char activePlayer, winner;
    int move, row, column;
    char playAgain;
    string filename;

    //////////////////////////////
    //File Reader Creation 

    cout << "Read from a file? Y or N?: ";
    char fileChoice;
    cin >> fileChoice;

    if (fileChoice == 'Y') {
        cout << "Filename: ";
        cin >> filename;
        ifstream file(filename.c_str());

        //////////////////////////////
        // File Reader Logic

        if (file) {
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    file >> board[i][j];
                }
            }
            //////////////////////////////
            // Task 5 File Reader

            if (file) {
                board[0][0] = '1'; board[0][1] = '2'; board[0][2] = '3';
                board[1][0] = '4'; board[1][1] = '5'; board[1][2] = '6';
                board[2][0] = '7'; board[2][1] = '8'; board[2][2] = '9';

                char player;
                int square;

                while (file >> player >> square) {
                    row = (square - 1) / 3;
                    column = (square - 1) % 3;
                    board[row][column] = player;
                }

                //////////////////////////////
                //Determining Winner

                file.close();
                winner = declareWinner(board);
                if (winner != 'N') {
                    cout << "Winner: " << winner << endl;
                }

                else {
                    cout << "Draw" << endl;
                }
                return 0;
            }
            else {
                return 1;
            }
        }
        else {
            return 1;
        }
    }
    else {
        //////////////////////////////
        // Determining Active Player Sequencing 

        cout << "Starting player (X or O): ";
        cin >> activePlayer;

        //////////////////////////////
        // Game Max Time Loop 

        for (int turn = 0; turn < 9; turn++) {
            playBoardVisual(board);

            //////////////////////////////
            //Player Input Directions

            cout << "Player " << activePlayer << " enter a number (1-9): ";
            cin >> move;

            //////////////////////////////
            //Error Clauses

            if (move < 1 || move > 9) {
                cout << "Not Possible, Try again." << endl;
                turn--;
                continue;
            }

            row = (move - 1) / 3;
            column = (move - 1) % 3;

            if (board[row][column] == 'X' || board[row][column] == 'O') {
                cout << "Not Possible. Try again." << endl;
                turn--;
                continue;
            }

            //////////////////////////////
            //Determining Winner

            board[row][column] = activePlayer;
            winner = declareWinner(board);

            if (winner != 'N') {
                playBoardVisual(board);
                cout << "Winner: " << winner << endl;
                break;
            }

            activePlayer = (activePlayer == 'X') ? 'O' : 'X';
        }

        if (winner == 'N') {
            cout << "Draw" << endl;
        }

        //////////////////////////////
        // Endgame Prompt

        cout << "Shall we play another game? (Y/N): ";
        cin >> playAgain;
        if (playAgain == 'Y') {
            main();
        }
    }

    return 0;
}