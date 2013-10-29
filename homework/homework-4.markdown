---
title: Homework 4
layout: default
---

Your task is to evaluate expressions like `3.4-(2.6+5.0)` and
`sin(-1.0)*2.0` and `-9.4^log(exp(cos(4.5)-2.3+3.5))`. These
expressions will be represented in "parse trees" such as the
following:

<pre>
3.4-(2.6+5.0) in tree form:

    -
   / \
  /   \
3.40   +
      / \
     /   \
    /     \
  2.60   5.00
</pre>

<pre>
sin(-1.0)*2.0 in tree form:

          *
         / \
        /   \
      sin  2.00
      /
     -
    / \
   /   \
  /     \
0.00   1.00
</pre>

<pre>
-9.4^log(exp(cos(4.5)-2.3+3.5)) in tree form:

       -
      / \
     /   \
   0.00   ^
         / \
        /   \
      9.40  log
            /
          exp
          /
         +
        / \
       /   \
      -   3.50
     / \
    /   \
  cos  2.30
  /
4.50
</pre>

These trees are arbitrarily large; your program should evaluate the
expression represented in any such tree. Your algorithm must be
recursive (for practice and because recursion is the most effective
technique for processing trees).

## Tree structure

You must use the following structure to represent a tree. **Note: do
not add this to your `main.cpp`; it is already inside `math_tree.h`
which you will already have.**

{% highlight cpp %}
class Tree
{
public:
    std::string op;
    double val;
    Tree *left;
    Tree *right;
};
{% endhighlight %}

Notice that this is a recursive definition. Every "node" in the
tree (for example, in the first tree above, the nodes are `-`, `+`,
`5.00`, `3.40`, and `2.60`) is itself a tree. Take any node, such as
`+`: now you have another tree (`+` at the top with `2.60` on the left
and `5.00` on the right).

Each "node" simply needs to store information about the operation
(e.g. `+`, `-`, `sin`) or the value (e.g. `3.40`) in addition to
pointers to the left and right subtrees. A node cannot be both a value
and an operation. We'll use the following convention to determine if a
node is an operation or just a value: the `op` variable should only be
non-empty if the node is an operation; otherwise, the node is a value
and the value is stored in `val`. You can test if the `op` variable is
empty, in a `Tree` pointer variable named `root`, with the following
expression: `if(root->op.empty())`

Here is how the tree at the beginning of this homework can be
represented:

{% highlight cpp %}
Tree m;
m.op = "-";
Tree p;
p.op = "+";
Tree v1;
v1.val = 3.4;
Tree v2;
v2.val = 2.6;
Tree v3;
v3.val = 5.0;

m.left = &v1;
m.right = &p;

p.left = &v2;
p.right = &v3;

v1.left = v1.right = v2.left = v2.right = v3.left = v3.right = NULL;
{% endhighlight %}

Note that operations (like '+') always have two subtrees; functions
(like `sin`) have only left subtrees, where the `right` pointer is
always `NULL`. For values, both `left` and `right` pointers are always
`NULL`.

## Automatic parsing

Writing trees "by hand" in your code is very difficult and does not
allow the user to interactively provide new expressions. In order to
allow user input, we need to take a string like "3.4-(2.6+5.0)" and
turn it into the right tree structure. This task is known as
"parsing," and can be quite difficult to code if you don't use the
right tools. I have coded a parser for you to use in your program. To
do so, I used the flex and bison tools, which allow me to describe a
parser in a special programming language, and which then produce C++
code that actually performs the parsing. If you want to learn more
about this, feel free to ask me.

## Set up your editor

You need to download some files (.cpp files and .h files) and
load these as your CodeBlocks/Visual Studio/Xcode "project."

Here is a ZIP package with all the files you need:
[hw4.zip](/homework/hw4.zip)

In there you'll also find `main.cpp` &mdash; use that file to begin
writing your own code. You'll see some instructions in the file. In
particular, you will see a loop that allows the user to type an
expression and have it evaluated (you won't need to modify that loop;
if you provide the rest of the code, the "interactive" parsing feature
should work properly).

I have created two videos for setting up Windows IDEs. Watch the
[CodeBlocks video](/video/homework-4-codeblocks.html) or the
[VisualStudio video](/video/homework-4-visualstudio.html).

## Your task

As mentioned, the `main.cpp` file found in the zip package has some
code but is missing some important functions. You must write the code
for these functions, *and* you must "hand-code" a tree structure at
the beginning of the `main()` function (look at the comments in the
file).

The functions you need to code are the following:

{% highlight cpp %}
double evalOp(string op, double val)
{
    // for evaluating functions like sin, cos, etc.
}

double evalOp(string op, double val1, double val2)
{
    // for evaluating operators like +, -, etc.
}

double eval(Tree *root)
{
    // for (recursively) evaluating a tree; this function will refer
    // back to itself (for evaluating subtrees) and will use the
    // evalOp() functions
}
{% endhighlight %}

The `eval()` function is the primary evaluation function; it accepts a
tree pointer and determines its value in a recursive fashion. The
other two `evalOp` functions, which have the same name but different
parameters, apply operators (like '+') or functions (like 'sin') to
their parameters (`val1` and/or `val2`).

Your program must support the following operators: `+`, `-`, `*`, `/`,
and `^` (e.g. `3^2` equals `9`). Your program must also support the
following functions: `sin`, `cos`, `tan`, `log`, `abs`. Add support
for more functions if you wish.

## User interface

Here is a sample interaction with your program:

<pre>
Enter expression: 2^3^4

        ^
       / \
      /   \
     ^   4.00
    / \
   /   \
  /     \
2.00   3.00

Result: 4096

Enter expression: log(2.71828^8)

      log
      /
     ^
    / \
   /   \
  /     \
2.72   8.00

Result: 7.99999

Enter expression: sin(tan(cos(sin(tan(cos(3.14159))))))

            sin
            /
          tan
          /
        cos
        /
      sin
      /
    tan
    /
  cos
  /
3.14

Result: 0.564596

Enter expression: sin(4.0)/cos(4.0)

       /
      / \
     /   \
    /     \
  sin     cos
  /       /
4.00    4.00

Result: 1.15782

Enter expression: tan(4.0)

  tan
  /
4.00

Result: 1.15782

Enter expression: sin(3.14159 / 2.0) + cos(3.14159 * 2.0)

              +
             / \
            /   \
           /     \
          /       \
         /         \
        /           \
      sin           cos
      /             /
     /             *
    / \           / \
   /   \         /   \
  /     \       /     \
3.14   2.00   3.14   2.00

Result: 2
</pre>


## Final words

When you give me your code, you only need to send me `main.cpp`,
unless you happened to have modified other files as well.
