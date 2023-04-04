# Prog 5: Unscrambler

[**Link to original specification page**](https://sites.google.com/view/uiccs-141fall2021/programs/prog-5-unscrambler?authuser=0)

Create a board that is filled with words which are then scrambled and presented to the user, who rotates the rows and columns to try and get the original words displayed in the right order on the board. Write a C++ program to do this. 

Random numbers are used throughout the program, so when you run it on your own machine versus interactively within Zybooks the output will likely be different, since the sequence of random numbers will be different. On the other hand, running the test cases should give you the same output every time. Read the notes below very carefully to minimize frustration that occurs when the sequence of random numbers generated is different from what is expected. Also be sure to include srand(1), and only call it once within your program.

Update 11/9/21 - The test cases are available on Zybooks.

Update 11/10/21 - The extra credit is described below, and examples are shown in the sample output. There is also a test case on Zybooks for the extra credit.

## 

Notes

1.  There is no starter code so think carefully about the steps needed to write the program, and how it can be built piece by piece.
    
2.  While you don't _have to_ use C++ classes for this program, you _will_ have to for program 6, which builds off this program, so if you do it now, you will save yourself some work later.
    
3.  The board is a square of characters, read sequentially from left to right and top to bottom. The size of the board is chosen by the user, and must be greater than or equal to 4. If the user enters a number smaller than 4, you must prompt them to input another number until a proper board size is entered.
    
4.  Rotation of each row shifts characters around, wrapping horizontally around the edges to the opposite side on the same row. Similarly rotating a column wraps characters around vertically to the opposite side in the same column. Rotation wraps around the board, so the last character ends up in the first spot, and each subsequent character is shifted by one unit. If the rotation is by a negative amount, the characters are shifted in the opposite direction.
    

```
Example of rotating a row by 1:       b e l o w ==>  w b e l o

Example of rotating a row by -2:      b e l o w ==> l o w b e

Example of rotating a column by 1:    b       w

                                      e       b

                                      l  ==>  e

                                      o       l

                                      w       o

Example of rotating a column by -1:   b       e

                                      e       l

                                      l  ==>  o

                                      o       w

                                      w       b
```



Random numbers are used throughout the program, so when you run it on your own machine versus interactively within Zybooks the output will likely be different, since the sequence of random numbers will be different. On the other hand, running the test cases should give you the same output every time. Read the notes below very carefully to minimize frustration that occurs when the sequence of random numbers generated is different from what is expected. Also be sure to include srand(1), and only call it once within your program.

5.  Generate the board as follows:
    
    1.  First select words at random from the ~14,000 word dictionary of words that are all 3-5 characters long. Each word on the board is separated by a single space after it. You may end up with 1 or 2 spaces at the end of the board, because the smallest length available is 3 and thus no words will fit in that space. To ensure your output matches the expected output on Zybooks, continue getting random words until the board is filled, stopping when there are 0, 1 or 2 spaces left, where no new word would fit. Thus if there are 3 spaces left and you find a random word that has length 4 or 5, you would continue to find a new random word until you find one that is of length 3. Similarly if there are 4 spaces left and you find a random word that is length 5, you would continue to find a new random word until you find one that is of length 3 or 4.
        

E.g. If the randomly selected words in order were _bear, bat, and eagle_ then this would fill up a 4x4 board as shown below. Notice that there are 2 spaces after eagle because there are no words in the dictionary that will fit in that space:
```
 ---------------
| b | e | a | r |
 ---------------
|   | b | a | t |
 ---------------
|   | e | a | g |
 ---------------
| l | e |   |   |
 ---------------
```



  2.  After the board is full with words selected at random, the board is scrambled. The extent of board scrambling depends on the scrambling number which the user provides that must be greater than or equal to 1. If the user enters a number smaller than 1, prompt them to input another number until a value >= 1 is entered.
        

Given user input _X_ for the number of times that the scrambling will occur, _X_ rows and _X_ columns are rotated by 1, alternating between row and column.

If the user enters 1 for the extent of scrambling, then first a randomly selected row is rotated by 1, and then a randomly selected column is rotated by 1. Alternatively if the user enters 2 for the extent of scrambling, then the above process is repeated twice, for a total of 4 rotations among the rows and columns of the board as follows:

            1.  A random row is rotated by 1.
                
            2.  A random column is rotated by 1.
                
            3.  A random row is rotated by 1.
                
            4.  A random column is rotated by 1.
                

Each random selection chooses from among all the options, so subsequent random rows/columns _could_ end up being the same as the previous one.

6.  To display the board in the format expected, each character in the row is separated by the '|' character to "draw" the columns. Each row is separated by a line of '-' characters, and the number of '-' characters displayed in each row is determined by the formula **boardSize*4** **- 1**. After the board is formatted in this way, there should be a line after it that prints the board as a single connected string. For the _bear-bat-eagle_ words on the 4x4 board this string would be those words separated by spaces, with two spaces at the end to complete the 16 characters, giving: "`bear bat eagle `". See the sample output for an example of this.
    
7.  There are a total of 6 menu options, as shown below. If the user enters a character that does not match any of these, print an error message and prompt them to choose again. See the sample output below for more details on what this should look like when running the program.
    

-   **R** to **R**otate a row or column.
    
    -   -   After the user selects this option, they should receive another prompt which asks them which row or column they would like to rotate, and how much they would like to rotate it by. Their response should be in the format of <R or C> <row/column number> <number of positions to rotate>, where capitalization and whitespace are ignored. The number of positions to rotate can be positive or negative.
            
        -   The row or column number should be checked to ensure that it is a valid row/column in the board. For a board of size _n_ the maximum rotation in either direction should be _n-1_. If the user does not enter a valid row/column or does not format it correctly, an appropriate error message should be printed, and the sub-menu asking for the rotation information should be presented again. See the sample output below for more details on what this should look like when running the program.
            
-   **C** to view what the **C**ompleted board should look like. This would be the unscrambled board.
    
-   **B** to reset the board back to the **B**eginning. The beginning state of the board is the board after scrambling, but before the user rotates any rows or columns.
    
-   **G** to **G**enerate a new board with new random words. This should use the same board size and difficulty that the user specified at the beginning of the game. If they would like a new board size and/or difficulty, they would need to rerun the program.
    
-   **S** to automatically **S**olve the board for 5 points of extra credit. The extra credit consists of having the computer solve the board on its own. Prompt for and read in from the user a string representing a board of the current size. The program must then find the completed board. You may assume the following:
    
    -   The board has been scrambled with a value of 1, 2, or 3, but this will **not** match the scramble value that the user typed in at the start of the program. (It may, but this is not guaranteed, nor do you need to enforce it).
        
    -   The user has entered all the characters in the board correctly, including spaces in between words as well as spaces at the beginning and end of the board.
        
    -   The board is of the same size that the user typed in at the start of the program. You do not need to enforce this, but can assume correct output.
        

See the end of the sample output below for details on what solving the board looks like when you run the program.

-   **Q** to **Q**uit.
    

8.  If the user successfully unscrambles the board, the program should display the completed board and a congratulatory message saying "`Congratulations, you won! Thank you for playing!`" Then print "`Exiting program...`" and the program should stop running.
