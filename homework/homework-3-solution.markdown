---
title: Homework 3 Solution
layout: default
---

{% highlight cpp %}
#include <iostream>
#include <windows.h>
using namespace std;

void display(char grid[22][80])
{
    // Print top of border
    cout << "+";
    for(int j = 0; j < 80; j++)
        cout << "-";
    cout << "+";
    cout << endl;

    // For each row
    for(int i = 0; i < 22; i++)
    {
        // Print border on left side
        cout << "|";
        // For each column
        for(int j = 0; j < 80; j++)
        {
            cout << grid[i][j];
        }
        // Print border on right side
        cout << "|" << endl;
    }

    // Print bottom of border
    cout << "+";
    for(int j = 0; j < 80; j++)
        cout << "-";
    cout << "+";
    cout << endl;
}

// Count neighbors for some particular cell
int count_neighbors(char grid[22][80], int row, int col)
{
    int count = 0;

    // up
    if(row > 0 && grid[row-1][col] == '*') count++;
    // down
    if(row < 21 && grid[row+1][col] == '*') count++;
    // left
    if(col > 0 && grid[row][col-1] == '*') count++;
    // right
    if(col < 79 && grid[row][col+1] == '*') count++;
    // up-left
    if(row > 0 && col > 0 && grid[row-1][col-1] == '*') count++;
    // up-right
    if(row > 0 && col < 79 && grid[row-1][col+1] == '*') count++;
    // down-left
    if(row < 21 && col > 0 && grid[row+1][col-1] == '*') count++;
    // down-right
    if(row < 21 && col < 79 && grid[row+1][col+1] == '*') count++;

    return count;
}

void generation(char grid[22][80])
{
    // Make a copy of the grid called "gridcopy" so that we can count
    // neighbors in the copy while modifying the original grid; if you
    // count neighbors in and modify the same grid, the game won't
    // work.
    char gridcopy[22][80];
    // Need to copy every cell individually
    for(int i = 0; i < 22; i++)
    {
        for(int j = 0; j < 80; j++)
        {
            gridcopy[i][j] = grid[i][j];
        }
    }

    for(int i = 0; i < 22; i++)
    {
        for(int j = 0; j < 80; j++)
        {
            // it's important to give the count_neighbors function "gridcopy"
            int neighbors = count_neighbors(gridcopy, i, j);

            // birth when a dead cell has three neighbors
            if(gridcopy[i][j] != '*' && neighbors == 3)
            {
                grid[i][j] = '*';
            }
            // loneliness or overcrowding, cell dies
            if(gridcopy[i][j] == '*' && (neighbors < 2 || neighbors > 3))
            {
                grid[i][j] = ' ';
            }
        }
    }
}

int main()
{
    // Create an initial blank grid
    char grid[22][80];
    for(int i = 0; i < 22; i++)
    {
        for(int j = 0; j < 80; j++)
        {
            grid[i][j] = ' ';
        }
    }

    // Turn on some initial cells
    grid[14][45] = '*';
    grid[14][46] = '*';
    grid[15][44] = '*';
    grid[15][45] = '*';
    grid[16][45] = '*';

    while(true)
    {
        display(grid);
        generation(grid);
        Sleep(800); // pause 800 ms
    }

    return 0;
}
{% endhighlight %}
