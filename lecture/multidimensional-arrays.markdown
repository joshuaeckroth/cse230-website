---
title: Multidimensional arrays
layout: default
---

<div id="toc">
[TOC]
</div>

Arrays are perfect for representing a collection of values that can be thought
of as a single dimension of data. But for two-dimensional data, we cannot just
create many different one-dimensional arrays. A two-dimensional (or
higher-dimensional) data set *can* be finagled into a one-dimensional array
(with clever trickery using array indices) but this is inconvenient. In order
to conveniently represent 2D data, we will use 2D arrays, also known as
matrices.

What kinds of data are found in matrices? Pictures are 2D grids of pixels, and
thus are represented as matrices. Spreadsheets (like those made in Microsoft
Excel) are matrices. As alluded to in class, generic "graph" structures can be
represented in matrices (in what are called "adjacent matrices"). Temperature
readings at every hour over several days could be saved in a matrix.

## Creating a 2D array

Here is the basic technique for creating a 2D array. In this example, the
matrix is 10 rows by 10 columns. All of its "cells" are set to 0.0.

{% highlight cpp %}
double arr[10][10];
for(int i = 0; i < 10; i++)
{
    for(int j = 0; j < 10; j++)
    {
        arr[i][j] = 0.0;
    }
}
{% endhighlight %}

For smaller matrices, the values can be set individually:

{% highlight cpp %}
double xyz[3][2] = { {0.0, 1.0}, {5.0, 6.0}, {11.0, 12.0} };
{% endhighlight %}

## Passing 2D arrays to functions

Unfortunately, passing 2D arrays to functions does not work as you might hope.
This is not a valid function header:

{% highlight cpp %}
// NOT CORRECT!!
int sumAllColumns(int arr[][], int rows, int cols)
{
    // ...
}
{% endhighlight %}

In order to pass a 2D array to a function, the number of columns must be known:

{% highlight cpp %}
// CORRECT
int sumAllColumns(int arr[][10], int rows)
{
    // ...
}
{% endhighlight %}

Of course, the 10 above (referring to the number of columns) may be different
in a different program. The basic idea is that if the array is created with a
size (rows and columns) not known until the file is read (the file specifies
the size), then the array cannot be passed to a function.

Here is an example of passing a 2D array to a function that prints the contents
of the array in a grid format.

{% highlight cpp %}
#include <iostream>
using namespace std;

void print_2d_array(char arr[3][4])
{
    for(int i = 0; i < 3; i++)
    {
        cout << "[";
        for(int j = 0; j < 4; j++)
        {
            cout << arr[i][j];
        }
        cout << "]" << endl;
    }
    cout << endl << endl;
}

int main()
{
    char myarray[3][4] = { {'a', 'b', 'c', 'd'},
                           {'e', 'f', 'g', 'h'},
                           {'i', 'j', 'k', 'l'} };
    print_2d_array(myarray);

    return 0;
}
{% endhighlight %}

That example uses a fixed, pre-defined array size. Here is a similar example in
which the array size is not known ahead of time. We use a "double pointer"
syntax to indicate the input to the function is a 2D array, whose size is
provided by the other two function parameters.

{% highlight cpp %}
#include <iostream>
using namespace std;

void print_2d_array(char **arr, int rows, int cols)
{
    for(int i = 0; i < rows; i++)
    {
        cout << "[";
        for(int j = 0; j < cols; j++)
        {
            cout << arr[i][j];
        }
        cout << "]" << endl;
    }
    cout << endl << endl;
}

int main()
{
    int r, c;
    cout << "Enter rows and cols: ";
    cin >> r >> c;

    // make the array of arrays
    char **myarray = new char*[r];
    for(int i = 0; i < r; i++)
    {
        // for each row, make an array of 'c' chars
        myarray[i] = new char[c];
    }

    // fill up array from user input
    for(int i = 0; i < r; i++)
    {
        cout << "Provide " << c << " chars to put in array "
             << "row " << (i+1) << ": ";
        for(int j = 0; j < c; j++)
        {
            cin >> myarray[i][j];
        }
    }

    print_2d_array(myarray, r, c);

    // delete each row array
    for(int i = 0; i < r; i++)
    {
        delete [] myarray[i];
    }
    // then delete the array of rows
    delete [] myarray;

    return 0;
}
{% endhighlight %}

Here is a sample execution:

<pre>
Enter rows and cols: 4 3
Provide 3 chars to put in array row 1: a b c
Provide 3 chars to put in array row 2: d e f
Provide 3 chars to put in array row 3: g h i
Provide 3 chars to put in array row 4: j k l
[abc]
[def]
[ghi]
[jkl] 
</pre>

## A matrix in a one-dimensional array

Above, it was mentioned that with some "clever trickery" with array indices, a
2D array could be stored in a 1D array. We will investigate in this section.

Since a matrix is just a grid with some fixed number of rows and columns, we
can "linearize" the grid by just joining each row end-to-end. In a 10x10
matrix, there are 100 cells. So we can make a 1D array with 100 cells and use
indices appropriately to access individual elements as if they were in a 10x10
grid.

The math is as follows. Assume our matrix has *r* rows and *c* columns. If an
element was a row *i* and column *j* in the original matrix, then in the 1D
array the element is at index <i>i*c+j</i>.

This program reads values from a file and stores them into a matrix. The first
two values in the file indicate the number of rows and columns of the matrix of
data that appears in the file. The program outputs the sums of the columns.

{% highlight cpp %}
#include <iostream>
#include <fstream>
using namespace std;

int main()
{
    string filename;
    ifstream file;
    cout << "Enter file name: ";
    getline(cin, filename);

    file.open(filename.c_str());
    if(!file.is_open())
    {
        cout << "Error opening file." << endl;
        return -1;
    }

    int numRows, numCols;
    file >> numRows >> numCols;

    int *arr = new int[numRows * numCols];

    // read each value into the array
    for(int i = 0; i < numRows; i++)
    {
        for(int j = 0; j < numCols; j++)
        {
            file >> arr[i * numCols + j];
        }
    }

    file.close();

    // find the sum for each column;
    // note that we can look through the array
    // in any order; we will look at columns first
    int sum;
    for(int j = 0; j < numCols; j++)
    {
        sum = 0;
        for(int i = 0; i < numRows; i++)
        {
            sum += arr[i * numCols + j];
        }
        cout << "Sum for column " << j << " = " << sum << endl;
    }

    delete [] arr;

    return 0;
}
{% endhighlight %}

Since this new approach, representing 2D matrices as 1D arrays, uses plain ol'
arrays, we can pass these plain ol' 1D arrays to functions, just like before:

{% highlight cpp %}
int sumAllColumns(int arr[], int rows, int cols) // CORRECT
{
    // ...
}
{% endhighlight %}

## Matrix multiplication example

This example uses "fake" 2D arrays; really 1D arrays are used, but they are
treated like 2D arrays. This example also shows "templates" although we will
not learn about these for a few more weeks.

{% highlight cpp %}
#include <iostream>
using namespace std;

int pos(int row, int col, int cols)
{
    return (row * cols + col);
}

template<typename T>
void matrixmult(T A[], T B[], T C[], int rows,
                int colsInner, int colsOuter)
{
    for(int i = 0; i < rows; i++)
    {
        for(int j = 0; j < colsOuter; j++)
        {
            C[pos(i,j,colsOuter)] = 0;
            for(int k = 0; k < colsInner; k++)
            {
                C[pos(i,j,colsOuter)] += A[pos(i,k,colsInner)]
                                       * B[pos(k,j,colsOuter)];
            }
        }
    }
}

template<typename T>
void printMatrix(T m[], int rows, int cols)
{
    for(int i = 0; i < rows; i++)
    {
        cout << "[\t";
        for(int j = 0; j < cols; j++)
        {
            cout << m[pos(i,j,cols)] << "\t";
        }
        cout << "]" << endl;
    }
}

int main()
{
    int rows, colsInner, colsOuter, temp;
    cout << "Enter matrix A rows/cols (separated by space): ";
    cin >> rows >> colsInner;

    cout << "Enter matrix B rows/cols (separated by space): ";
    cin >> temp >> colsOuter;
    if(temp != colsInner)
    {
        cout << "Error, A (" << rows << "x" << colsInner
             << ") cannot mulitply B ("
             << temp << "x" << colsOuter << ") because "
             << temp << " != " << colsInner
             << "." << endl;
        return 0;
    }

    // make these arrays of doubles if you like;
    // the matrixmult function can handle
    // int arrays or double arrays
    int *A = new int[rows*colsInner];
    int *B = new int[colsInner*colsOuter];
    int *C = new int[rows*colsOuter];

    cout << "Now enter the matrix values." << endl;
    for(int i = 0; i < rows; i++)
    {
        cout << "A's row " << (i+1) << " (" << colsInner
             << " values with spaces): ";
        for(int j = 0; j < colsInner; j++)
        {
            cin >> A[pos(i,j,colsInner)];
        }
    }
    cout << endl;
    for(int i = 0; i < colsInner; i++)
    {
        cout << "B's row " << (i+1) << " (" << colsOuter
             << " values with spaces): ";
        for(int j = 0; j < colsOuter; j++)
        {
            cin >> B[pos(i,j,colsOuter)];
        }
    }
    cout << endl;

    cout << "Got matrix A:" << endl;
    printMatrix(A, rows, colsInner);
    cout << endl;

    cout << "Got matrix B:" << endl;
    printMatrix(B, colsInner, colsOuter);
    cout << endl;

    matrixmult(A, B, C, rows, colsInner, colsOuter);

    cout << "A * B:" << endl;
    printMatrix(C, rows, colsOuter);

    delete [] A;
    delete [] B;
    delete [] C;

    return 0;
}
{% endhighlight %}

## Matrix multiplication with the MTL package

MTL is available here: http://www.simunova.com/en/node/24

{% highlight cpp %}
#include <iostream>
#include <boost/numeric/mtl/mtl.hpp>
using namespace std;
using namespace mtl;

int main()
{
    int rows, colsInner, colsOuter, temp;
    cout << "Enter matrix A rows/cols (separated by space): ";
    cin >> rows >> colsInner;

    cout << "Enter matrix B rows/cols (separated by space): ";
    cin >> temp >> colsOuter;
    if(temp != colsInner)
    {
        cout << "Error, A (" << rows << "x" << colsInner
             << ") cannot mulitply B ("
             << temp << "x" << colsOuter << ") because "
             << temp << " != " << colsInner
             << "." << endl;
        return 0;
    }

    dense2D<double> A(rows, colsInner);
    dense2D<double> B(colsInner, colsOuter);
    dense2D<double> C(rows, colsOuter);

    cout << "Now enter the matrix values." << endl;
    for(int i = 0; i < rows; i++)
    {
        cout << "A's row " << (i+1) << " (" << colsInner
             << " values with spaces): ";
        for(int j = 0; j < colsInner; j++)
        {
            cin >> A[i][j];
        }
    }
    cout << endl;
    for(int i = 0; i < colsInner; i++)
    {
        cout << "B's row " << (i+1) << " (" << colsOuter
             << " values with spaces): ";
        for(int j = 0; j < colsOuter; j++)
        {
            cin >> B[i][j];
        }
    }
    cout << endl;

    cout << "Got matrix A:" << endl;
    cout << A << endl;

    cout << "Got matrix B:" << endl;
    cout << B << endl;

    cout << "A * B:" << endl;
    C = A * B;
    cout << C << endl;

    return 0;
}
{% endhighlight %}

## Improved matrix code

{% highlight cpp %}
#include <iostream>
#include <iomanip> // for printout (set precision, etc.)
#include <cassert> // for assert() commands
using namespace std;

// make a "structure" that has two ints and
// an array of double values;
// call this structure (using 'typedef')
// by the name "matrix"
typedef struct {
    int rows;
    int cols;
    double *vals;
} matrix;

// make a new matrix, return it;
// matrices must later be deleted with
// the mat_del() function below
matrix mat_new(int rows, int cols)
{
    matrix m;
    m.rows = rows;
    m.cols = cols;
    m.vals = new double[rows * cols];
    return m;
}

// new matrices must be deleted when
// they are no longer needed
void mat_del(matrix m)
{
    delete [] m.vals;
}

// set a matrix value at i,j
void mat_set(matrix m, int i, int j, double val)
{
    assert(i >= 0 && i < m.rows && j >= 0 && j < m.cols);
    m.vals[m.cols * i + j] = val;
}

// get a matrix value at i,j
double mat_get(matrix m, int i, int j)
{
    assert(i >= 0 && i < m.rows && j >= 0 && j < m.cols);
    return m.vals[m.cols * i + j];
}

// fancy display of a matrix
void mat_disp(matrix m)
{
    cout.precision(4);
    cout.setf(ios::fixed, ios::floatfield);

    for(int i = 0; i < m.rows; i++)
    {
        cout << "[";
        for(int j = 0; j < m.cols; j++)
        {
            cout << setw(10) << m.vals[m.cols * i + j] << "  ";
        }
        cout << "]";
        cout << endl;
    }
    cout << endl << endl;
}

// simplistic matrix multiplication;
// this is the slowest method;
// returns a new matrix (which must be
// deleted lated with mat_del())
matrix mat_mult(matrix m1, matrix m2)
{
    assert(m1.cols == m2.rows);
    matrix m3 = mat_new(m1.rows, m2.cols);

    for(int i = 0; i < m1.rows; i++)
    {
        for(int j = 0; j < m2.cols; j++)
        {
            mat_set(m3, i, j, 0.0);

            for(int k = 0; k < m1.cols; k++)
            {
                double prior = mat_get(m3, i, j);
                double m1_val = mat_get(m1, i, k);
                double m2_val = mat_get(m2, k, j);
                mat_set(m3, i, j, prior + m1_val * m2_val);
            }
        }
    }
    return m3;
}

// perform a test
int main()
{
    matrix m1 = mat_new(2, 3);
    mat_set(m1, 0, 0, 3.4);
    mat_set(m1, 0, 1, 6.2);
    mat_set(m1, 0, 2, 3.8);
    mat_set(m1, 1, 0, 7.3);
    mat_set(m1, 1, 1, 38.2);
    mat_set(m1, 1, 2, -2.1);

    matrix m2 = mat_new(3, 2);
    mat_set(m2, 0, 0, 4.6);
    mat_set(m2, 0, 1, 10.2);
    mat_set(m2, 1, 0, -203.3);
    mat_set(m2, 1, 1, -30.3);
    mat_set(m2, 2, 0, 0.2);
    mat_set(m2, 2, 1, 0.0);

    mat_disp(m1);

    cout << "times:" << endl;

    mat_disp(m2);

    cout << "equals:" << endl;

    matrix m3 = mat_mult(m1, m2);

    mat_disp(m3);

    /* Matlab code to check that example:

        m1 = [3.4, 6.2, 3.8; 7.3, 38.2, -2.1]
        m2 = [4.6, 10.2; -203.3, -30.3; 0.2, 0.0]
        m1 * m2
    */

    mat_del(m1);
    mat_del(m2);
    mat_del(m3);

    return 0;
}
{% endhighlight %}

