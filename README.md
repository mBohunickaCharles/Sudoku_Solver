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
To learn more about the history of Sudoku, visit this [!website](https://www.sudokuconquest.com/blog/a-brief-history-of-sudoku).
<br><br/>


## Backtracking

Backtracking is a versatile algorithm used to solve problems involving puzzles, games, optimization, and combinatorics. It works by building solutions step by step, allowing previous choices to be reversed if they lead to an incorrect or suboptimal outcome.

Backtracking can be seen as a type of depth-first search that explores one branch of the solution space at a time, retreating to a previous decision point whenever it hits a dead end or finds a better alternative.

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

To solve our sudoku from above we are going to use backtracking algorithm and python programming language. Before we jump to the solution we need to represent sudoku in python. We will asign a list of nine sublists to a sudoku variable. Each sublist represents a horizontal line of our sudoku puzzle. We replaced missing value that needs to be filled by 0.

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

Here, we define a fancy ```print_sudoku()``` function that provides us with nicer grid when we print sudoku in python. 
Another option to achieve similar print option is to ```import numpy as np``` and just simply use ```print(np.matrix(sudoku))```. 

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

To initiate recusrion within our backtracking algorithm, we define constrainst. Those are our puzzle rules that define if the choise of digit is possible. We are basically checking for any duplicated values of 1 to 9 digits in row, column and 3x3 subgrid:

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

Checking entries in rows and columns is straidforward. However when it comes to checking boxes we assign to each subgrid member the same staring value, depending on it's position in the 9x9 grid. To achieve this we use a floor division and multiply our otput by 3. New values assigned to rows and columns are displayed on the image below:
<p align="center">
<img width="350em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_square.drawio.png" align = "center"/>
</p>
<br><br/>

Function ```solve()``` represents our backtracking algorithm for solving sudoku. Here we follow a recusive structure, where each recursive call represents a step in the solution. The algorithm starts searching for possible digits from top left to bottom right, it maintains state variables that keep track of the current partial solution, the remaining options and the constraints ```possible()```. If it comes to clush with constrainst and no options/digits can be filled in algorithm perfoms recusion - goes step back and replaces digit to another possible option.

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




