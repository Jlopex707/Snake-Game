//***********************
//|     SNAKE GAME      |
//***********************
#include <iostream>
#include <conio.h>
#include <windows.h>
#include <time.h> // To make spawn value more random
using namespace std;
bool gameOver;
const int width = 20;
const int height = 20;
int x, y, pelletX, pelletY, score;
int tailX[100], tailY[100];
int nTail;
enum eDirecton { STOP = 0, LEFT, RIGHT, UP, DOWN };
eDirecton dir;
void Setup()
{
    gameOver = false;
    dir = STOP;
    x = width / 2;
    y = height / 2;
    pelletX = rand() % width;
    pelletY = rand() % height;
    score = 0;
}
void Draw()
{
    system("cls"); //system("clear");
    cout << endl;
    cout << " ";
    for (int i = 0; i < width + 2; i++)
        cout << "_"; //Upper boarder
    cout << endl;

    for (int i = 0; i < height; i++)
    {
        for (int j = 0; j < width; j++)
        {
            if (j == 0)
                cout << "|"; //Side boarder
            if (i == y && j == x)
                cout << "o"; //Head
            else if (i == pelletY && j == pelletX)
                cout << "*"; //Pellet 
            else
            {
                bool print = false;
                for (int k = 0; k < nTail; k++)
                {
                    if (tailX[k] == j && tailY[k] == i)
                    {
                        cout << "x"; //Tail
                        print = true;
                    }
                }
                if (!print)
                    cout << " ";
            }

            if (j == width - 1)
                cout << "|"; //Side boarder
        }
        cout << endl;
    }

    cout << " ";
    for (int i = 0; i < width + 2; i++)
        cout << "-"; //Bottom boarder
    cout << endl;
    cout << " Score:" << score << endl; //Score counter
}
void Input()
{
    if (_kbhit())
    {
        switch (_getch())
        {
        case 'a':
            dir = LEFT; //"a" to go left.
            break;
        case 'd':
            dir = RIGHT; //"d" to go right.
            break;
        case 'w':
            dir = UP; //"w" to go up.
            break;
        case 's':
            dir = DOWN; //"s" to go down.
            break;
        case 'x':
            gameOver = true;
            break;
        default:
            break;
        }
    }
}
void Logic()
{
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;
    for (int i = 1; i < nTail; i++)
    {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }
    switch (dir)
    {
    case LEFT:
        x--;
        break;
    case RIGHT:
        x++;
        break;
    case UP:
        y--;
        break;
    case DOWN:
        y++;
        break;
    default:
        break;
    }
    if (x > width || x < 0 || y > height || y < 0)
        gameOver = true;
    //if (x >= width) x = 0; else if (x < 0) x = width - 1;
    //if (y >= height) y = 0; else if (y < 0) y = height - 1;

    for (int i = 0; i < nTail; i++)
        if (tailX[i] == x && tailY[i] == y)
            gameOver = true;

    if (x == pelletX && y == pelletY)
    {
        srand(time(0)); // Random seed value for rand based on time
        score += 10;
        pelletX = rand() % width;
        pelletY = rand() % height;
        nTail++;
    }
}
int main()
{
    system("MODE con cols=24 lines=25");
    Setup();
    while (!gameOver)
    {
        Draw();
        Input();
        Logic();
        Sleep(100); //Higher value imapcts speed.
    }
    return 0;
    //Make sure you are not on caps lock.

}
