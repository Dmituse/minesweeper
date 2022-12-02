#include <iostream>
#include <string>
#include <time.h>
using namespace std;

	int ShowField(int (&arr)[10][10])
	{
		for (int i = 0; i < 10; i++)
		{
			cout << endl;
			for (int j = 0; j < 10; j++)
			{
			cout << arr[i][j] << " ";
			}
		}
		return 0;
	}

	int FillFieldOfBombs(int (&fieldOfMines)[10][10], int const mines)
	{
		srand(time(NULL));
		for (int i = 0; i < mines; i++)
		{
		tryToFillAgain:
			int rand1 = rand() % 10;
			int rand2 = rand() % 10;
			if (fieldOfMines[rand1][rand2] == 9)
			{
				goto tryToFillAgain;
			}
			else
			{
				fieldOfMines[rand1][rand2] = 9;
			}
		}
		return 0;
	}

	int CellCount(int(&fieldOfMines)[10][10], int(&numberOfField)[10][10])
	{
		for (int i = 0; i < 10; i++)
		{
			for (int j = 0; j < 10; j++)
			{
				if (i == 0 && j == 0)
				{
					numberOfField[i][j] = (fieldOfMines[i][j + 1] + fieldOfMines[i + 1][j] + fieldOfMines[i + 1][j + 1]) / 9;
				}
				else if (i == 0 && j == 9)
				{
					numberOfField[i][j] = (fieldOfMines[i][j - 1] + fieldOfMines[i + 1][j - 1] + fieldOfMines[i + 1][j]) / 9;
				}
				else if (i == 9 && j == 0)
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j] + fieldOfMines[i - 1][j + 1] + fieldOfMines[i][j + 1]) / 9;
				}
				else if (i == 9 && j == 9)
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j - 1] + fieldOfMines[i - 1][j] + fieldOfMines[i][j - 1]) / 9;
				}
				else if (i == 0)
				{
					numberOfField[i][j] = (fieldOfMines[i][j - 1] + fieldOfMines[i][j + 1] + fieldOfMines[i + 1][j - 1] + fieldOfMines[i + 1][j] + fieldOfMines[i + 1][j + 1]) / 9;
				}
				else if (i == 9)
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j - 1] + fieldOfMines[i - 1][j] + fieldOfMines[i - 1][j + 1] + fieldOfMines[i][j - 1] + fieldOfMines[i][j + 1]) / 9;
				}
				else if (j == 0)
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j] + fieldOfMines[i - 1][j + 1] + fieldOfMines[i][j + 1] + fieldOfMines[i + 1][j] + fieldOfMines[i + 1][j + 1]) / 9;
				}
				else if (j == 9)
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j - 1] + fieldOfMines[i - 1][j] + fieldOfMines[i][j - 1] + fieldOfMines[i + 1][j - 1] + fieldOfMines[i + 1][j]) / 9;
				}
				else
				{
					numberOfField[i][j] = (fieldOfMines[i - 1][j - 1] + fieldOfMines[i - 1][j] + fieldOfMines[i - 1][j + 1] + fieldOfMines[i][j - 1] + fieldOfMines[i][j + 1] + fieldOfMines[i + 1][j - 1] + fieldOfMines[i + 1][j] + fieldOfMines[i + 1][j + 1]) / 9;
				}
			}
		}
		return 0;
	}

	int CheckFreeZone(int(&numberOfField)[10][10], int(&fieldOfView)[10][10], int i, int j)
	{
		if (i == 0 && j == 0 && numberOfField[i][j] == 0)
		{
			fieldOfView[i][j + 1] = numberOfField[i][j + 1];
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
			fieldOfView[i + 1][j + 1] = numberOfField[i + 1][j + 1];
			if (fieldOfView[i + 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j+1);
			}
		}
		else if (i == 0 && j == 9 && numberOfField[i][j] == 0)
		{
			fieldOfView[i][j - 1] = numberOfField[i + 1][j - 1];
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i + 1][j - 1] = numberOfField[i + 1][j - 1];
			if (fieldOfView[i + 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j-1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
		}			
		else if (i == 9 && j == 0 && numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j] = numberOfField[i - 1][j]; 
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i - 1][j + 1] = numberOfField[i - 1][1 + 1]; 
			if (fieldOfView[i - 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j+1);
			}
			fieldOfView[i][j + 1] = numberOfField[i][j + 1]; 
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
		}
		else if (i == 9 && j == 9 && numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j - 1] = numberOfField[i - 1][j - 1];		
			if (fieldOfView[i - 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j-1);
			}
			fieldOfView[i - 1][j] = numberOfField[i - 1][j];		
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i][j - 1] = numberOfField[i][j - 1];		
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
		}
		else if (i == 0 && numberOfField[i][j] == 0)
		{
			fieldOfView[i][j - 1] = numberOfField[i][j - 1]; 	
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i][j + 1] = numberOfField[i][j + 1];	
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
			fieldOfView[i + 1][j - 1] = numberOfField[i + 1][j - 1];	
			if (fieldOfView[i + 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j-1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];		
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
			fieldOfView[i + 1][j + 1] = numberOfField[i + 1][j + 1];	
			if (fieldOfView[i + 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j+1);
			}
		}
		else if (i == 9 && numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j - 1] = numberOfField[i - 1][j - 1];
			if (fieldOfView[i - 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i - 1][j] = numberOfField[i - 1][j];		
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i - 1][j + 1] = numberOfField[i - 1][j + 1];	
			if (fieldOfView[i - 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j+1);
			}
			fieldOfView[i][j - 1] = numberOfField[i][j - 1];		
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i][j + 1] = numberOfField[i][j + 1];		
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
		}
		else if (j == 0 && numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j] = numberOfField[i - 1][j];	
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i - 1][j + 1] = numberOfField[i - 1][j + 1];		
			if (fieldOfView[i - 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j+1);
			}
			fieldOfView[i][j + 1] = numberOfField[i][j + 1];			
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];		
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
			fieldOfView[i + 1][j + 1] = numberOfField[i + 1][j + 1];	
			if (fieldOfView[i + 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j+1);
			}
		}
		else if (j == 9 && numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j - 1] = numberOfField[i - 1][j - 1];	
			if (fieldOfView[i - 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j-1);
			}
			fieldOfView[i - 1][j] = numberOfField[i - 1][j];		
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i][j - 1] = numberOfField[i][j - 1];		
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i + 1][j - 1] = numberOfField[i + 1][j - 1];	
			if (fieldOfView[i + 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j-1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];		
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
		}
		else if (i!=0&&i!=9&&j!=0&&j!=9&&numberOfField[i][j] == 0)
		{
			fieldOfView[i - 1][j - 1] = numberOfField[i - 1][j - 1];
			if (fieldOfView[i - 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j-1);
			}
			fieldOfView[i - 1][j] = numberOfField[i - 1][j];		
			if (fieldOfView[i - 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j);
			}
			fieldOfView[i - 1][j + 1] = numberOfField[i - 1][j + 1];	
			if (fieldOfView[i - 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i-1, j+1);
			}
			fieldOfView[i][j - 1] = numberOfField[i][j - 1];		
			if (fieldOfView[i][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j-1);
			}
			fieldOfView[i][j + 1] = numberOfField[i][j + 1];		
			if (fieldOfView[i][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i, j+1);
			}
			fieldOfView[i + 1][j - 1] = numberOfField[i + 1][j - 1];	
			if (fieldOfView[i + 1][j - 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j-1);
			}
			fieldOfView[i + 1][j] = numberOfField[i + 1][j];			
			if (fieldOfView[i + 1][j] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j);
			}
			fieldOfView[i + 1][j + 1] = numberOfField[i + 1][j + 1];	
			if (fieldOfView[i + 1][j + 1] == 0)
			{
				CheckFreeZone(numberOfField, fieldOfView, i+1, j+1);
			}
		}
		else
	{
	fieldOfView[i][j] = numberOfField[i][j];
	}
		
		return 0;
	}

	int WayToWin(int(&fieldOfView)[10][10], int finish, int k)
	{
		k = 0;
		for (int i = 0; i < 10; i++)
		{
			for (int j = 0; j < 10; j++)
			{
				if (fieldOfView[i][j] == 9)
				{
					k++;
				}
			}
		}
		return k;
	}

int main()
{
	setlocale(LC_ALL, "ru");

    int fieldOfMines[10][10] = { {0} };
	int numberOfField[10][10] = { {0} };
	int fieldOfView[10][10];
	for (int i = 0; i < 10; i++)
	{
		for (int j = 0; j < 10; j++)
		{
			fieldOfView[i][j] = 9;
		}
	}
	int row, col;
	int finish;
	int mines;
	cout << "Выберите кол-во мин ";
	cin >> mines;
	finish = 100 - mines;
	FillFieldOfBombs(fieldOfMines, mines);
	ShowField(fieldOfMines);
	ShowField(fieldOfView);

Choice:

	cout << endl << "выберите строку ";
	cin >> row;
	cout << "выберите столбец ";
	cin >> col;
	int k = 0;
	if (fieldOfMines[row-1][col-1] == 9)
	{
		goto GameOver;
	}
	else
	{
		CellCount(fieldOfMines, numberOfField);
		CheckFreeZone(numberOfField, fieldOfView, row - 1, col - 1);
		ShowField(fieldOfView);
		WayToWin(fieldOfView, finish, k);
		if (k == finish)
		{
			goto Finish;
		}
		goto Choice;
	}
GameOver:
	cout << "game over" << endl;
Finish:
	cout << "Поздравляем, это победа!";
}

