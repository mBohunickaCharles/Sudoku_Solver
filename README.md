# Solving Sudoku by using Backtracking  <br><br/>

Sudoku, a puzzle game formerly called Number Place, challenges players with logic and number-based combinations.

Below is an example of a Sudoku puzzle, which we will solve using the backtracking algorithm in Python.

<p align="center">
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_1.drawio.png" align = "center"/>
</p>
<br><br/>

We’ll be working on a classic 9x9 Sudoku puzzle, which is made up of nine 3x3 subgrids. The objective is to fill each empty cell with a number from 1 to 9, without repeating any number in the same row, column, or subgrid.

<p align="center">
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/horizontal.png" align = "center"/>
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/vertical.png" align = "center"/>
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/subgrid.png" align = "center"/>  
</p>

To learn more about the history of Sudoku, visit this [website](https://www.sudokuconquest.com/blog/a-brief-history-of-sudoku).
<br><br/>


## Backtracking

Backtracking is a versatile algorithm used to solve problems involving puzzles, games, optimization, and combinatorics. It works by building solutions step by step, allowing previous choices to be reversed if they lead to an incorrect or suboptimal outcome.

It can be seen as a type of depth-first search that explores one branch of the solution space at a time, retreating to a previous decision point whenever it hits a dead end or finds a better alternative.

Pseudocode of backtracking algorithm:
```python
def backtrack(x):
    if x is not a solution:
        return false
    if x is a new solution:
        add to list of solutions
    backtrack(expand x)
 ```   
<br><br/>

### Solving Sudoku puzzle by Backtracking <br><br/>

To solve the Sudoku puzzle above, we’ll use the backtracking algorithm along with the Python programming language. Before diving into the solution, we need to represent the Sudoku grid in Python. We’ll assign a list of nine sublists to a variable called sudoku, where each sublist represents a horizontal row of the puzzle. Missing values that need to be filled are represented by 0.

```python
sudoku = [
    [5,7,0,0,4,6,0,3,0],
    [0,6,0,0,2,0,0,0,0],
    [8,0,0,5,7,3,0,9,0],
    [0,9,5,0,8,0,3,1,0],
    [0,0,8,4,6,9,7,0,0],
    [7,2,0,3,0,0,0,0,0],
    [0,5,0,6,0,0,0,7,3],
    [0,0,7,8,1,0,9,2,0],
    [0,0,0,0,3,4,1,6,0]
]
```
<br><br/>

Here, we define a fancy ```print_sudoku()``` function that provides us with a nicer grid when we print sudoku in Python. 
Another option to achieve nicer print is to use ```import numpy as np``` or ```print(np.matrix(sudoku))```. 

```python
def print_sudoku():
    global sudoku
    for row in range(9):
        if row % 3 == 0 and row != 0:
            print("- - - - - - - - - - - -")

        for col in range(9):
            if col % 3 == 0 and col != 0:
                print(" | ", end = "")

            if col == 8:
                print(sudoku[row][col])
            else:
                print(str(sudoku[row][col]) + " ", end = "")
```


```python
print_sudoku()

5 7 0  | 0 4 6  | 0 3 0
0 6 0  | 0 2 0  | 0 0 0
8 0 0  | 5 7 3  | 0 9 0
- - - - - - - - - - - -
0 9 5  | 0 8 0  | 3 1 0
0 0 8  | 4 6 9  | 7 0 0
7 2 0  | 3 0 0  | 0 0 0
- - - - - - - - - - - -
0 5 0  | 6 0 0  | 0 7 3
0 0 7  | 8 1 0  | 9 2 0
0 0 0  | 0 3 4  | 1 6 0
```
<br><br/>

Before we start the recursive backtracking process, we need to set up the rules, or constraints, of the puzzle. These rules help us decide if placing a certain number is allowed. We check if the number already exists in the same row, column, or 3x3 box—if it does, the move isn’t valid.



```python
def possible(row, col, n):
    global sudoku
    
    # Checking row:
    for i in range(9):
        if sudoku[row][i] == n:
            return False

    # Checking column:
    for j in range(9):
        if sudoku[j][col] == n:
            return False

    # Checking box
    row_0 = (row // 3) * 3
    col_0 = (col // 3) * 3

    for i in range(0,3):
        for j in range(0,3):
            if sudoku[row_0 + i][col_0 + j] == n:
                return False
            
    return True
```

Checking for duplicate entries in rows and columns is straightforward. However, when it comes to 3x3 subgrids, we assign the same starting index to each cell within a subgrid based on its position in the 9x9 grid. To do this, we use floor division and multiply the result by 3. The resulting values for rows and columns are shown in the image below.

<p align="center">
<img width="350em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_square.drawio.png" align = "center"/>
</p>
<br><br/>

The function ```solve()``` implements our backtracking algorithm to solve the Sudoku puzzle. It follows a recursive structure, where each recursive call represents a step in the solution process. The algorithm starts from the top-left corner and moves toward the bottom-right, maintaining state variables to track the current partial solution, remaining options, and constraints through the possible() function. If the algorithm encounters a conflict with the constraints and cannot fill in any digits, it backtracks by stepping back and trying a different option.


It also contains a base case, where it checks if the solution is complete and valid and prints out the solution if any.

```python
def solve():
    global sudoku
    
    for row in range(9):
        for col in range(9):
            if sudoku[row][col] == 0:
                for n in range(1,10):
                    if possible(row,col,n):
                        sudoku[row][col] = n
                        if solve():                  # recursion
                            return True
                        sudoku[row][col] = 0         # backtracking
                return False
    return True

if solve():
    print("Sudoku solved:")
    print_sudoku()
else:
    print('No solution found!')
```                        
<br><br/>

```python
Sudoku solved:
5 7 1  | 9 4 6  | 2 3 8
9 6 3  | 1 2 8  | 5 4 7
8 4 2  | 5 7 3  | 6 9 1
- - - - - - - - - - - -
4 9 5  | 2 8 7  | 3 1 6
3 1 8  | 4 6 9  | 7 5 2
7 2 6  | 3 5 1  | 4 8 9
- - - - - - - - - - - -
1 5 4  | 6 9 2  | 8 7 3
6 3 7  | 8 1 5  | 9 2 4
2 8 9  | 7 3 4  | 1 6 5
```




