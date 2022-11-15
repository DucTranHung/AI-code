#include <bits/stdc++.h>
using namespace std;
#define nmax 1000007
#define mmax 5007
int scoring[2][nmax], optimal[2][nmax];
int board[mmax][mmax];
bool inRange(int x, int l, int r)   {return (x >= l && x <= r);}
bool minimize(int &a, int b)        {if (a > b) {a = b; return 1;} return 0;}
bool maximize(int &a, int b)        {if (a < b) {a = b; return 1;} return 0;}
void printBoard()
{
    for (int i = 0; i <= 2; i++)
    {
        cout << "-------------\n";
        for (int j = 0; j <= 2; j++)
        {
            cout << "|";
            if (board[i][j] == 0) cout << " O ";
            if (board[i][j] == 1) cout << " X ";
            if (board[i][j] == 2) cout << "   ";
        }
        cout << "|\n";
    }
    cout << "-------------";
}
int state()
{
    int cur = 0;
    for (int i = 0; i <= 8; i++)
    {
        int row = i / 3;
        int col = i % 3;
        int coef = board[row][col];
        cur = 3 * cur + (coef);
    }
    return cur;
}
int Finish()
{
    for (int i = 0; i <= 2; i++)
        if (board[i][0] == board[i][1] && board[i][1] == board[i][2] && board[i][0] != 2) return board[i][0];
    for (int i = 0; i <= 2; i++)
        if (board[0][i] == board[1][i] && board[1][i] == board[2][i] && board[0][i] != 2) return board[0][i];
    if (board[0][0] == board[1][1] && board[1][1] == board[2][2] && board[0][0] != 2) return board[0][0];
    if (board[0][2] == board[1][1] && board[1][1] == board[2][0] && board[2][0] != 2) return board[2][0];
    int dem = 0;
    for (int i = 0; i <= 2; i++)
        for (int j = 0; j <= 2; j++) dem += (board[i][j] != 2);
    return (dem == 9 ? 2 : 3);
}
void calculateMove(bool role, int firstRole)
{
    int Res = Finish();
    int currentBoard = state();
    if (Res != 3)
    {
        scoring[firstRole][currentBoard] = (Res == 2 ? 0 : 1 - 2 * Res);
        return;
    }
    scoring[firstRole][currentBoard] = (!role ? -3 : 3);
    for (int i = 0; i <= 8; i++)
    {
        int row = i / 3;
        int col = i % 3;
        if (board[row][col] != 2) continue;
        board[row][col] = role;
        int newBoard = state();
        calculateMove(!role, firstRole);
        if (!role)
        {
            if (maximize(scoring[firstRole][currentBoard], scoring[firstRole][newBoard]))
                optimal[firstRole][currentBoard] = newBoard;
        }
        else
        {
            if (minimize(scoring[firstRole][currentBoard], scoring[firstRole][newBoard]))
                optimal[firstRole][currentBoard] = newBoard;
        }
        board[row][col] = 2;
    }
}
void prepare()
{
    for (int i = 0; i <= 2; i++)
        for (int j = 0; j <= 2; j++) board[i][j] = 2;
    calculateMove(0, 0);
    for (int i = 0; i <= 2; i++)
        for (int j = 0; j <= 2; j++) board[i][j] = 2;
    calculateMove(1, 1);
    for (int i = 0; i <= 2; i++)
        for (int j = 0; j <= 2; j++) board[i][j] = 2;
}
vector <int> extract(int nowState)
{
    vector <int> ans;
    for (int i = 0; i <= 8; i++)
    {
        ans.push_back(nowState % 3);
        nowState /= 3;
    }
    reverse(ans.begin(), ans.end());
    return ans;
}
void followBestMove(int firstRole)
{
    int curState = state();
    vector <int> nowState = extract(curState);
    vector <int> newState = extract(optimal[firstRole][curState]);
    for (int i = 0; i <= 8; i++)
    {
        if (nowState[i] == newState[i]) continue;
        int row = i / 3;
        int col = i % 3;
        board[row][col] = newState[i];
    }
}
char decideRole;
int role;
void solve()
{
    cout << "You will be \"X\" and the computer will be \"O\".\n";
    cout << "Who goes first?        You (Y)/ The computer (C)\n";
    cin >> decideRole;
    role = (decideRole == 'Y');
    cout << (role ? "You" : "The computer") << " will go first.\n";
    int curRole = role;
    prepare();
    while (1)
    {
        int judge = Finish();
        if (judge != 3)
        {
            cout << "The game ended. ";
            if (judge == 1) cout << "You won.";
            else if (judge == 0) cout << "The computer won.";
            else cout << "It's a draw.";
            break;
        }
        if (curRole != role)
            cout << (curRole % 2 == 1 ? "It's your turn." : "It's the computer's turn.");
        if (curRole % 2 == 1)
        {
            cout << "\nInsert your move: ";
            int col = 0, row = 0;
            cin >> row >> col;
            if (!inRange(row, 0, 2) || !inRange(col, 0, 2) || board[row][col] != 2)
            {
                cout << "\nInvalid move. The game is canceled.\n";
                break;
            }
            board[row][col] = 1;
            cout << "\nYou played your move.\n \n";
        }
        else
        {
            followBestMove(role);
            cout << "\nThe computer played.\n \n";
        }
        curRole++;
        printBoard();
        cout << '\n';
    }
}
int main()
{
    ios_base::sync_with_stdio(false);
    int test = 1;
//    cin >> test;
    char decision;
    while (test--)
    {
        solve();
        cout << "Wanna play again?      Yes(Y)/ No(N)";
        cin >> decision;
        if (decision == 'Y') test++;
        else cout << "Oh well!.";
    }
    return 0;
}


