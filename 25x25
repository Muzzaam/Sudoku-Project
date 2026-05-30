#include <iostream>
#include <vector>
#include<map>
#include<set>
#include <chrono>
#pragma GCC optimize("Ofast")
#pragma GCC optimize("Ofast,unroll-loops")
#pragma GCC optimize "-funroll-loops"
#pragma GCC target("avx,avx2,fma")



using namespace std;
const int n = 25;
const int m = 5;

vector<vector<int>> sudoku(n, vector<int>(n));
vector<vector<set<int>>> possiblities(n, vector<set<int>>(n, set<int>()));

int position_calculate (int number){
    int correct = (number/m)*m;
    return correct;
}

void updateChoices(int &row, int &col, int value)
{
    
    sudoku[row][col] = value;
    possiblities[row][col].clear();

    #pragma GCC ivdep
    for (int r = 0; r < n; ++r)
    {
        possiblities[row][r].erase(value);
        possiblities[r][col].erase(value);
    }
    int SubCol = position_calculate(col);
    int SubRow = position_calculate(row);
    
    for (int r = 0; r < m; r++)
    {
        for (int c = 0; c < m; c++)
        {
            int posR = SubRow + r;
            int posC = SubCol + c;
            possiblities[posR][posC].erase(value);
        }
    }
}

bool checkmachine(int y, int x, int space) {
    #pragma GCC ivdep
    for (int i = 0; i < n; i++) {
        if (sudoku[y][i] == space) {
            return false;
        }
        if (sudoku[i][x] == space) {
            return false;
        }
    }

    int x0 = position_calculate(x);
    int y0 = position_calculate(y);
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < m; j++) {
            int newY = y0 +i;
            int newX = x0 +j;
            if (sudoku[newY][newX] == space) {
                return false;
            }
        }
    }
    return true;
}

void getChoices(vector<vector<int>>& puzzle)
{
    #pragma GCC ivdep
    for (int row = 0; row < n; ++row)
    {
        for (int col = 0; col < n; ++col)
        {
            set<int> &numbers = possiblities[row][col];
            if (puzzle[row][col] == 0) 
            {
                for (int num = 1; num < n + 1; ++num)
                {
                    if (checkmachine(row, col, num))
                    {
                        numbers.insert(num);
                    }
                }
            }
        }
    }
}


void hiddenSingles()
{
    bool foundHS;
    #pragma GCC ivdep
    do
    {
        foundHS = false;

        #pragma GCC ivdep
        for (int row = 0; row < n; ++row)
        {
            int HSMapRow[n + 1] = {0};
            for (int col = 0; col < n; ++col)
            {
                int numChoices = possiblities[row][col].size();
                if (!sudoku[row][col] && numChoices > 0)
                {
                    for (int num : possiblities[row][col])
                    {
                        HSMapRow[num]++;
                    }
                }
            }
            for (int num = 1; num <= n; ++num)
            {
                if (HSMapRow[num] == 1)
                {
                    for (int col = 0; col < n; ++col)
                    {
                        if (possiblities[row][col].count(num) == 1)
                        {
                            updateChoices(row, col, num);
                            foundHS = true;
                            break;
                        }
                    }
                }
            }
        }

        #pragma GCC ivdep
        for (int col = 0; col < n; ++col)
        {
            int HSMapCOL[n + 1] = {0};
            for (int row = 0; row < n; ++row)
            {
                int numChoices = possiblities[row][col].size();
                if (!sudoku[row][col] && numChoices > 0)
                {
                    for (int num : possiblities[row][col])
                    {
                        HSMapCOL[num]++;
                    }
                }
            }
            for (int num = 1; num <= n; ++num)
            {
                if (HSMapCOL[num] == 1)
                {
                    for (int row = 0; row < n; ++row)
                    {
                        if (possiblities[row][col].count(num) == 1)
                        {
                            updateChoices(row, col, num);
                            foundHS = true;
                            break;
                        }
                    }
                }
            }
        }

        #pragma GCC ivdep
        for (int box = 0; box < n; box++)
        {
            int Subrows = (box / m) * m;
            int subcols = (box % m) * m;
            int HSMapSUB[n + 1] = {0};
            for (int row = 0; row < m; ++row)
            {
                for (int col = 0; col < m; ++col)
                {
                    int SumR = row + Subrows;
                    int SumC = col + subcols;
                    int numChoices = possiblities[SumR][SumC].size();
                    if (!sudoku[SumR][SumC] && numChoices > 0)
                    {
                        for (int num : possiblities[SumR][SumC])
                        {
                            HSMapSUB[num]++;
                        }
                    }
                }
            }
            for (int num = 1; num <= n; ++num)
            {
                if (HSMapSUB[num] == 1)
                {
                    for (int row = 0; row < m; ++row)
                    {
                        for (int col = 0; col < m; ++col)
                        {
                            int k = row + Subrows;
                            int p = col + subcols;
                            if (possiblities[k][p].count(num) == 1)
                            {
                                updateChoices(k, p, num);
                                foundHS = true;
                                break;
                            }
                        }
                    }
                }
            }
        }
        

       
    } while (foundHS);
}


void nakedsingles(){
    #pragma GCC ivdep
    for (int i = 0; i < n; i++) {
         for (int j = 0; j < n; j++) {
             if (possiblities[i][j].size() == 1) {
                updateChoices(i, j, *(possiblities[i][j].begin()));
                  i = 0;
                  j = 0;
             }
         }
     }
}

bool solvedfully = false;
bool truesol(int &NEWROW, int &NEWCOL , vector<vector<int>>& puzzle)
{
    for (int row = 0; row < n; ++row)
    {
        for (int col = 0; col < n; ++col)
        {
            if (puzzle[row][col] == 0)
            {
                NEWROW = row;
                NEWCOL = col;
                return true;
            }
        }
    }
    return false;
}

bool solver(vector<vector<int>>& puzzle) {
    
    hiddenSingles();
    vector<vector<set<int>>> tmpchoice = possiblities;
    vector<vector<int>> tmpsudoku = sudoku;

    int NEWROW,NEWCOL;
    
    if (truesol(NEWROW,NEWCOL, sudoku) == false){
        solvedfully = true;
        return true;
    }


#pragma GCC ivdep
 for (int val : tmpchoice[NEWROW][NEWCOL]) {
        updateChoices(NEWROW, NEWCOL, val);
        

        if (solver(puzzle)) { 
            return true; 
        }

        sudoku[NEWROW][NEWCOL] = 0;
        possiblities = tmpchoice;
        sudoku = tmpsudoku;
        nakedsingles();
    }

    return false; 
}

int main() {

    #pragma GCC ivdep
    for (int row = 0; row < n; ++row) {
        for (int col = 0; col < n; ++col) {
            char inp;
            cin >> inp;

            if (inp >= 65 && inp <= 90) {
                sudoku[row][col] = inp - 55;
            } else {
                sudoku[row][col] = inp - '0';
            }
        }
    }

    getChoices(sudoku);
    //hiddenSingles();
    solver(sudoku);

        if (solvedfully){
        #pragma GCC ivdep
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 24) {
                    if (sudoku[i][j] >= 10 && sudoku[i][j] <= 25) {
                        cout << (char)('A' + sudoku[i][j] - 10);
                    }
                    else cout << sudoku[i][j];
                } 
                
                else {
                    if (sudoku[i][j] >= 10 && sudoku[i][j] <= 25) {
                        cout << (char)('A' + sudoku[i][j] - 10) << " ";
                    } else {
                        cout << sudoku[i][j] << " ";
                    }
                }
            }
            cout << endl;
        }
        }
        
     if (!solvedfully) {
        cout << "No Solution" << endl;
    }
   

    

    return 0;
}






