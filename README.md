## Solving Sudoku by using Backtracking  <br><br/>

Sudoku, originally called Number place is a logic based, combinatorical number game. We will be solving a classic sudoku game, that consists of 9x9 number grids that contains nine smaller 3x3 sections. The aim of the game is to fill in empty sqares by 1 to 9 digits, so there are no duplicated values in horizontal line, vertical line and smaller 3x3 square.

Below is an example of classic sudoku game, we are going to solve by using backtracking algorithm and python programming.

<p align="center">
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_1.drawio.png" align = "center"/>
</p>
<br><br/>

### Using Human Approach
Before we jump into algorithmical solution, let's have a closer look at human approach to solving this puzzle game. 
Firstly we scan the sudoku grid and decide where we can possibly fill in an empty square. This would usually be the sqare with most values given horizontally, vertically or in the smaller quare. I have highlighted my selected square by "?", hoping I can fill in this value with maximum confidence.

<p align="center">
<img width="250em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_2.drawio.png" align = "center"/>
</p>
<br><br/>

Looking at horizontal line I know that 3, 5, 6 and 7 can't be options replacing our "?".

<p align="center">
<img width="300em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_col.drawio.png" align = "center"/>
</p>
<br><br/>

Vertical line also excludes 1, 2 ,4, and 8. Since 1, 2, 3, 4, 5, 6, 7, 8 can't placed, the only option is 9. 

<p align="center">
<img width="300em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_row.drawio.png" align = "center"/>
</p>
<br><br/>

I check the square: It doesn't contain 9. Hence, I can now confidently replace "?" by 9. 

<p align="center">
<img width="300em" src="https://github.com/mBohunickaCharles/sudoku_solver/blob/main/image/sudoku_square_1.drawio.png" align = "center"/>
</p>
<br><br/>

To continue, I would now search for another empty square that can be replaced by digit more confidently than others. Approach is similar to the one described above.

### Using Backtracking


