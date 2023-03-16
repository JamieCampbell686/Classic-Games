#include <iostream>
#include <windows.h>
#include <string> 
#include <cstdlib>
using namespace std;

string response;
void menu();

struct TicTacToeData
{
    string PlayerLetter;
    string AILetter;
    bool FirstRound = false;
    bool GameWon = false;
    string Winner;
    string BoardData[9] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };
    void ActivateMenu()
    {
        system("CLS");
        cout << "================================\n\nWelcome to Tic Tac Toe!\n\n================================\n\nWould you like to start?\n>> ";
        cin >> response;
        for (int i = 0; i < response.length(); i++) {
            response[i] = tolower(response[i]);
        }
        if (response == "yes" or response == "y") {
            system("CLS");
            return;
        }
        if (response == "no" or response == "n") {
            exit(0);
        }
        else
        {
            cout << "That is not an option!\nPlease try inputting either yes or no." << endl;
            Sleep(3000);
            ActivateMenu();
        }
    }
    void PickLetter()
    {
        system("CLS");
        cout << "Please pick either X or O.\n>> ";
        cin >> response;
        for (int i = 0; i < response.length(); i++) {
            response[i] = tolower(response[i]);
        }
        if (response == "o") {
            PlayerLetter = "O";
            AILetter = "X";
            system("CLS");
            return;
        }
        if (response == "x") {
            PlayerLetter = "X";
            AILetter = "O";
            system("CLS");
            return;
        }
        else
        {
            cout << "That is not an option!" << endl;
            Sleep(3000);
            PickLetter();
        }
    }
    void DisplayBoard() {
        cout << " " << BoardData[0] << " | " << BoardData[1] << " | " << BoardData[2] << endl;
        cout << "---+---+---" << endl;
        cout << " " << BoardData[3] << " | " << BoardData[4] << " | " << BoardData[5] << endl;
        cout << "---+---+---" << endl;
        cout << " " << BoardData[6] << " | " << BoardData[7] << " | " << BoardData[8] << endl;
    }
    void ActivateAITurn()
    {
        bool temp = true;
        bool chosen = false;
        while (temp == true) {

            for (int i = 0; i <= 8; i++) {
                if (BoardData[i] == PlayerLetter and chosen == false) {
                    for (int x = 1; x <= 9; x++) {
                        if ((i - x) >= 0 and (i + x) <= 8 and chosen == false) {
                            if (BoardData[i - x] == PlayerLetter and BoardData[i + x] == to_string(i + x + 1) and chosen == false) {
                                BoardData[i + x] = AILetter;
                                chosen = true;
                                temp = false;
                            }
                            if (BoardData[i + x] == PlayerLetter and BoardData[i - x] == to_string(i - x + 1) and chosen == false) {
                                BoardData[i - x] = AILetter;
                                chosen = true;
                                temp = false;
                            }
                        }
                    }
                }
            }
            for (int i = 0; i < 9; i++) {
                if (BoardData[i] != to_string(i + 1)) {
                    i;
                    if (i == 8) {
                        cout << "All spaces are taken..." << endl;
                        chosen = true;
                        temp = false;
                        Sleep(2500);
                    }
                }
                else
                {
                    break;
                }
            }

            if (chosen == false) {
                srand((unsigned)time(NULL));
                int decision = (rand() % 8);
                if (BoardData[decision] != to_string(decision + 1)) {
                    temp;
                }
                else
                {
                    temp = false;
                    BoardData[decision] = AILetter;
                    system("CLS");
                    DisplayBoard();
                    cout << "Your opponent chose the number " << to_string(decision + 1) << "!" << endl;
                    Sleep(3000);
                }
            }
        }
    }
    void ActivatePLRTurn()
    {
        system("CLS");
        DisplayBoard();
        cout << "\nEnter the number in which you would like to place your letter:\n>> ";
        int response = 0;
        cin >> response;
        cin.clear();
        cin.ignore(10000, '\n');
        if ((10 - response) > 0 and (10 - response) < 10) {
            string StrResponse = to_string(response);
            if (BoardData[response - 1] != StrResponse) {
                cout << "That spot is already taken!\nEnter a spot that is not taken.";
                Sleep(3000);
                ActivatePLRTurn();
            }
            else
            {
                BoardData[response - 1] = PlayerLetter;
            }
        }
        else
        {
            cout << "Invalid response, enter again." << endl;
            Sleep(2000);
            ActivatePLRTurn();
        }
    }
    void CheckForWin()
    {
        for (int i = 0; i < 9; i++) {
            if (i == 0 or i == 1 or i == 2) {
                if (BoardData[i] == BoardData[i + 3] and BoardData[i + 3] == BoardData[i + 6]) {
                    GameWon = true;
                    if (BoardData[i] == PlayerLetter) {
                        Winner = "Player";
                    }
                    else {
                        Winner = "AI";
                    }
                }
            }
            if (i == 0 or i == 3 or i == 6 and GameWon == false) {
                if (BoardData[i] == BoardData[i + 1] and BoardData[i + 1] == BoardData[i + 2]) {
                    GameWon = true;
                    if (BoardData[i] == PlayerLetter) {
                        Winner = "Player";
                    }
                    else {
                        Winner = "AI";
                    }
                }
            }
            if (GameWon == false) {
                if (BoardData[0] == BoardData[4] and BoardData[4] == BoardData[8]) {
                    GameWon = true;
                    if (BoardData[0] == PlayerLetter) {
                        Winner = "Player";
                    }
                    else {
                        Winner = "AI";
                    }
                }
                if (BoardData[2] == BoardData[4] and BoardData[4] == BoardData[6]) {
                    GameWon = true;
                    if (BoardData[2] == PlayerLetter) {
                        Winner = "Player";
                    }
                    else {
                        Winner = "AI";
                    }
                }
            }
        }
        bool SpaceLeft = false;
        if (GameWon == false) {
            for (int i = 0; i <= 8; i++) {
                if (BoardData[i] == to_string(i + 1)) {
                    SpaceLeft = true;
                }
            }
            if (SpaceLeft == false) {
                GameWon = true;
                Winner = "NULL";
            }
        }

    }
    void StartGame(string FirstPlayer) {
        if (FirstPlayer == "player") {
            ActivatePLRTurn();
            system("CLS");
            CheckForWin();
            if (GameWon == false) {
                system("CLS");
                DisplayBoard();
                cout << "\nYour opponent is choosing a spot..." << endl;
                ActivateAITurn();
                CheckForWin();
            }
            if (GameWon == true) {
                if (Winner == "AI") {
                    cout << "The AI has got 3 in a row!\n\nThe AI won." << endl;
                }
                if (Winner == "Player") {
                    cout << "The Player has got 3 in a row!\n\nThe Player won." << endl;
                }
                if (Winner == "NULL") {
                    cout << "It was a tie!\n\nNo one won." << endl;
                }
            }
            else
            {
                StartGame("player");
            }
        }
        if (FirstPlayer == "AI") {
            system("CLS");
            DisplayBoard();
            cout << "\nYour opponent is choosing a spot..." << endl;
            ActivateAITurn();
            system("CLS");
            CheckForWin();
            if (GameWon == false) {
                ActivatePLRTurn();
                CheckForWin();
            }
            if (GameWon == true) {
                if (Winner == "AI") {
                    cout << "The AI has got 3 in a row!\n\nThe AI won." << endl;
                }
                if (Winner == "Player") {
                    cout << "The Player has got 3 in a row!\n\nThe Player won." << endl;
                }
                if (Winner == "NULL") {
                    cout << "It was a tie!\n\nNo one won." << endl;
                }
            }
            else
            {
                StartGame("AI");
            }
        }
    }
};

struct RockPaperScissorsData {
    int PlayerGamesWon = 0;
    int AIGamesWon = 0;

    string PlayerAnswer;
    string AIAnswer;
    string Winner = "NULL";

    void StartMenu() 
    {
        system("CLS");
        Sleep(1000);
        cout << "-----------------------------\nWelcome to Rock Paper Scissors!\n-----------------------------\n\nWould you like to start?\n>> ";
        cin >> response;
        for (int i = 0; i < response.length(); i++) {
            response[i] = tolower(response[i]);
        }
        if (response == "yes" or response == "y") {
            
            StartGuesses();
        }
        if (response == "no" or response == "n") {
            cout << "\nEnding the game...";
            Sleep(2000);
            menu();
        }
        else {
            cout << "\nThat is not a valid answer!";
            Sleep(2000);
            StartMenu();
        }

    }
    bool repeat;
    void StartGuesses() 
    {
        repeat = true;
        while (repeat == true) {
            PlayerGuess();
            AIGuess();
            CheckForWin();
            system("CLS");
            cout << "\n\nPlayer Score: " << PlayerGamesWon << endl;
            cout << "AI Score: " << AIGamesWon << endl;
            Sleep(4000);
            if (PlayerGamesWon == 5 or AIGamesWon == 5) {
                if (PlayerGamesWon == 5) {
                    Winner = "Player";
                }
                if (AIGamesWon == 5) {
                    Winner = "AI";
                }
                if (Winner == "Player") {
                    system("CLS");
                    Sleep(1000);
                    cout << "The player has won the game!";
                    Sleep(2000);
                    repeat = false;
                }
                if (Winner == "AI") {
                    system("CLS");
                    Sleep(1000);
                    cout << "The AI has won the game!";
                    Sleep(2000);
                    repeat = false;
                }
            }
        }
        cout << "\n\nReturning to menu...";
        Sleep(1000);
        menu();
    }
    void AIGuess()
    {
        srand((unsigned)time(NULL));
        int decision = (rand() % 2);
        if (decision == 0) {
            AIAnswer = "rock";
        }
        if (decision == 1) {
            AIAnswer = "paper";
        }
        if (decision == 2) {
            AIAnswer = "scissors";
        }
        cout << "It is now the AI's turn...\nThey picked " << AIAnswer << "!";
        Sleep(2000);
    }
    void PlayerGuess()
    {
        system("CLS");
        cout << "Pick either Rock Paper or Scissors\n>> ";
        cin >> response;
        for (int i = 0; i < response.length(); i++) {
            response[i] = tolower(response[i]);
        }
        if (response == "rock") {
            PlayerAnswer = "rock";
        }
        if (response == "paper") {
            PlayerAnswer = "paper";
        }
        if (response == "scissors") {
            PlayerAnswer = "scissors";
        }
        if (response != "rock" and response != "paper" and response != "scissors") {
            cout << "That is not a valid answer!";
            Sleep(2000);
            PlayerGuess();
        }
    }
    void CheckForWin() {
        if (PlayerAnswer == "rock" and AIAnswer == "scissors") {
            PlayerGamesWon += 1;
        }
        if (PlayerAnswer == "paper" and AIAnswer == "rock") {
            PlayerGamesWon += 1;
        }
        if (PlayerAnswer == "scissors" and AIAnswer == "paper") {
            PlayerGamesWon += 1;
        }
        if (AIAnswer == "rock" and PlayerAnswer == "scissors") {
            AIGamesWon += 1;
        }
        if (AIAnswer == "paper" and PlayerAnswer == "rock") {
            AIGamesWon += 1;
        }
        if (AIAnswer == "scissors" and PlayerAnswer == "paper") {
            AIGamesWon += 1;
        }
        else {
            Winner = "NULL";
        }
    }
};

void RockPaperScissors() {
    RockPaperScissorsData Console;
    Console.StartMenu();
}

void TicTacToe()
{
    TicTacToeData Console;
    Console.ActivateMenu();
    Sleep(1000);
    Console.PickLetter();
    Sleep(1000);
    if (Console.PlayerLetter == "X") {
        cout << "X will go first (The player)\n" << endl;
        Sleep(2000);
        Console.StartGame("player");
    }
    else
    {
        cout << "X will go first (The AI)\n" << endl;
        Sleep(2000);
        Console.StartGame("AI");
    }
    Sleep(1000);
    system("CLS");
    cout << "The game has ended.\n\nReturning to menu..." << endl;
    Sleep(2000);
    menu();
}

void menu() {
    system("CLS");
    cout << "----------------------------------\nWelcome to many games!\n----------------------------------" << endl;
    cout << "What game would you like to play? There are 2 options.\n\n1. TicTacToe\n2. RockPaperScissors\n\n>> ";
    cin >> response;
    for (int i = 0; i < response.length(); i++) {
        response[i] = tolower(response[i]);
    }
    if (response == "1" or response == "tictactoe") {
        TicTacToe();
    }
    if (response == "2" or response == "rockpaperscissors") {
        RockPaperScissors();
    }
    else {
        cout << "That is not a valid response!" << endl;
        Sleep(1000);
        menu();
    }
}

int main()
{
    menu();
}

