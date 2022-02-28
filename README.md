/**********************
|     SNAKE GAME      |
**********************/
#include <iostream>
#include <conio.h>
#include <windows.h>
#include <time.h> // To make spawn value more random
using namespace std;
bool gameOver;
const int width = 20;
const int height = 20;
int x, y, pelletX, pelletY, score;
/* Structure Examples
struct {
 int x;
 int y;
}pellet;
*/
int tailX[100], tailY[100];  //2 Arrays. The x and y coordinates. They move in pairs.
int nTail;  //Tail length
/*struct {
 Int speed; 
 Int len; 
 Int x [snakesize]; 
 Int y [snakesize];
*/
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
                cout << "O"; //Head
            else if (i == pelletY && j == pelletX)
                cout << "*"; //Pellet 
            else
            {
                bool print = false;             //Draw the snake as it grows.
                for (int k = 0; k < nTail; k++)
                {
                    if (tailX[k] == j && tailY[k] == i)
                    {
                        cout << "o"; //Tail
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
    int prevX = tailX[0]; //Remember the previous x coordinate of the tail
    int prevY = tailY[0]; //Remember the previous y coordinate of the tail
    int prev2X, prev2Y;
    tailX[0] = x; // Follow the head
    tailY[0] = y; // Follow the head
    for (int i = 1; i < nTail; i++) //Will go to the length of the tail as it grows.
    {
        prev2X = tailX[i]; //Update/print the next segment of the tail.
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
    /*      |High Score Board|
    {
    FILE *fPtr;
    char HighScoreTable[100];
    char yourScore[Buffer_Size];

    fPtr = fopen("HighScoreBoard.txt", "a");
         {
         if (fPtr == NULL)
         printf("\nUnable to open '%s' \n",HighScoreTable.txt);
         printf("Please check whether file exists and you have write privilege.\n");
         exit(EXIT_FAILURE);
    printf(fPtr, "Enter Your Score:\n")   
    fflush(stdin);
    fgets(yourScore, Buffer_Size, stdin);
    fputs(yourScore, fPtr);
    fPtr = freopen("HighScoreBoard", "r", fPtr);
    fclose(fPtr);
    }
         |Validation Input|
     int yourScore, input;
     yourScore = scanf("%d", &input);
     while(yourScore !=1,1000)
     {
     printf("Invalid Score, try again: ");
     status = scanf("%d", &input);
     }

    */
    /*      |Error Logging|
    FILE *fp1;
    fp1 = fopen("/sys/class/gpio/export","w"); 
    if(fp1 == NULL){
        FILE *fpErr = fopen("/home/log.txt", "a");
        if(fpErr != NULL)
             fprintf(fpErr, "errno:%s - opening GPIO136 failed - line 739\n ", strerror(errno));
        close(fpErr);
        close(fp1);
        exit(1);
    {

            |Login|
    printf("Enter your username:\n");
    scanf("%s",&username);

    printf("Enter your password:\n");
    scanf("%s",&password);

         if(strcmp(username,"chaitu")==0){
         if(strcmp(password,"123")==0){

         printf("\nWelcome.Login Success!");


         else{ printf("\nwrong password");
         printf("\nwrong password");

         else
            printf("\nUser doesn't exist");
}
    */
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
