---
title: Multidimensional arrays
layout: default
---

The arrays we saw previously were one-dimensional arrays ("vectors" in
math terminology). A "matrix," in math terminology, is therefore a
two-dimensional array. How do we create two-dimensional arrays? We
actually have two options available to us, as will be described
below.

What kinds of data are found in matrices? Pictures are 2D grids of
pixels, and thus are represented as matrices. Spreadsheets (like those
made in Microsoft Excel) are matrices. Generic "graph" structures
(e.g. who your friends are, and who their friends are, etc.) can be
represented in matrices (in what are called "adjacent
matrices"). Temperature readings at every hour over several days could
be saved in a matrix.

## 2D arrays: two different strategies

Obviously, data is stored in your computer's memory and memory is not
two-dimensional (it is one-dimensional), so 2D arrays (and 3D arrays,
4D arrays, etc.) must somehow be saved in one dimension. Understanding
how this might work is essential to understanding how 2D arrays work
in C++.

The simplest way to store 2D data in a 1D space is to "flatten" the
matrix to a vector, where each row is stored just to the right of the
previous row:

![Flattened 2D array](/images/2d-array-linear.png "Flattened 2D array")

As you see in the diagram, the 2D array is really just a 1D
array. Since the compiler knows the number of columns, it is able to
figure out where each 2D value is stored in the 1D array. (Notice that
the number of rows is not needed in order to calculate the position in
1D of a 2D value.)

Compare this to the alternative approach, which is to construct an
array of pointers whose values point to separate (non-contiguous)
arrays representing the rows. So our 2D array with two rows and four
columns becomes three arrays: an array with two pointers, each
pointing to an array of four values. The array of two pointers is
pointing to each of the 2D rows.

![Dynamic 2D array](/images/2d-array-dynamic.png "Dynamic 2D array")

Notice that in this case, the compiler does not know the number of
columns, and does not need to know in order for us to access
particular values. To access a value, we first look at the "row
pointers," follow a row pointer to a particular "row array," then move
to the particular column we want in that row array.

Using this second strategy, we are also not limited to keeping the
column sizes equivalent. We can create "jagged arrays" where the
columns sizes differ. The compiler does not care how big the columns
are; we are expected to write code that respects the column sizes (the
compiler will not know if we are referring to a column that doesn't
exist).

## Creating a 2D array (first strategy)

Here is the basic technique for creating a 2D array with the first
strategy. In this example, the matrix is 10 rows by 10 columns. All of
its "cells" are set to 0.0.

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

## Creating a 2D array (second strategy)

Here we create the same array as above, using the `new` operator (and
followed by `delete` when we are done with the array).

{% highlight cpp %}
double **arr = new double*[10];
for(int i = 0; i < 10; i++)
{
    arr[i] = new double[10];
    for(int j = 0; j < 10; j++)
    {
        arr[i][j] = 0.0;
    }
}

// ... later, must delete our array to prevent memory leaks

// delete each "row array" first
for(int i = 0; i < 10; i++)
{
    delete [] arr[i];
}
// then delete the array of "row pointers"
delete [] arr;
{% endhighlight %}

## Passing 2D arrays to functions (first strategy)

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

We saw this earlier: the number of columns must be known in order for
`arr[i][j]` to refer to a particular value. This limitation is one
good reason to use the second strategy of 2D array creation.

Supposing you are using the first strategy, here is an example of
passing a 2D array to a function that prints the contents of the array
in a grid format. The example uses a fixed, pre-defined array size.

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

## Passing 2D arrays to functions (second strategy)

When we use the second strategy of 2D array creation, the primary
variable is actually a double-pointer. So we just need to give the
double-pointer to the function (as well as the array rows and columns
sizes so we know how much to loop):

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


<!--
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
// an array of double values
struct matrix {
    int rows;
    int cols;
    double *vals;
};

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

-->