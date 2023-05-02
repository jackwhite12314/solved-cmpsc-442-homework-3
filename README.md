Download Link: https://assignmentchef.com/product/solved-cmpsc-442-homework-3
<br>
<h1>Instructions</h1>

In this assignment, you will explore a number of games and puzzles from the perspectives of informed and adversarial search.

A skeleton file homework3-cmpsc442.py containing empty definitions for each question has been provided. Since portions of this assignment will be graded automatically, none of the names or function signatures in this file should be modified. However, you are free to introduce additional variables or functions if needed.

You may import definitions from any standard Python library, and are encouraged to do so in case you find yourself reinventing the wheel. If you are unsure where to start, consider taking a look at the data structures and functions defined in the collections, itertools, Queue, and random modules.

You will find that in addition to a problem specification, most programming questions also include a pair of examples from the Python interpreter. These are meant to illustrate typical use cases to clarify the assignment, and are not comprehensive test suites. In addition to performing your own testing, you are strongly encouraged to verify that your code gives the expected output for these examples before submitting.

You are strongly encouraged to follow the Python style guidelines set forth in <a href="https://legacy.python.org/dev/peps/pep-0008/">PEP 8</a>, which was written in part by the creator of Python. However, your code will not be graded for style.

<h1>1.   Tile Puzzle [40 points]</h1>

Recall from class that the Eight Puzzle consists of a <em>3 x 3</em> board of sliding tiles with a single empty space. For each configuration, the only possible moves are to swap the empty tile with one of its neighboring tiles. The goal state for the puzzle consists of tiles 1-3 in the top row, tiles 4-6 in the middle row, and tiles 7 and 8 in the bottom row, with the empty space in the lower-right corner.

In this section, you will develop two solvers for a generalized version of the Eight Puzzle, in which the board can have any number of rows and columns. We have suggested an approach similar to the one used to create a Lights Out solver in Homework 2, and indeed, you may find that this pattern can be abstracted to cover a wide range of puzzles. If you wish to use the provided GUI for testing, described in more detail at the end of the section, then your implementation must adhere to the recommended interface. However, this is not required, and no penalty will imposed for using a different approach.

A natural representation for this puzzle is a two-dimensional list of integer values between <em>0</em> and <em>r · c – 1</em> (inclusive), where <em>r</em> and <em>c</em> are the number of rows and columns in the board, respectively. In this problem, we will adhere to the convention that the <em>0</em>-tile represents the empty space.

<ol>

 <li><strong>[0 points] </strong>In the TilePuzzle class, write an initialization method __init__(self, board) that stores an input board of this form described above for future use. You additionally may wish to store the dimensions of the board as separate internal variables, as well as the location of the empty tile.</li>

 <li><strong>[0 points] </strong><em>Suggested infrastructure.</em></li>

</ol>

In the TilePuzzle class, write a method get_board(self) that returns the internal representation of the board stored during initialization.

&gt;&gt;&gt; p = TilePuzzle([[1, 2], [3, 0]])                                                           &gt;&gt;&gt; p = TilePuzzle([[0, 1], [3, 2]])

&gt;&gt;&gt; p.get_board()                                                                                      &gt;&gt;&gt; p.get_board()

[[1, 2], [3, 0]]                                                                                               [[0, 1], [3, 2]]

Write a top-level function create_tile_puzzle(rows, cols) that returns a new TilePuzzle of the specified dimensions, initialized to the starting configuration. Tiles <em>1</em> through <em>r · c – 1 </em>should be arranged starting from the top-left corner in row-major order, and tile <em>0</em> should be located in the lower-right corner.

&gt;&gt;&gt; p = create_tile_puzzle(3, 3)                                                            &gt;&gt;&gt; p = create_tile_puzzle(2, 4)

&gt;&gt;&gt; p.get_board()                                                                                      &gt;&gt;&gt; p.get_board()

[[1, 2, 3], [4, 5, 6], [7, 8, 0]]                                                                  [[1, 2, 3, 4], [5, 6, 7, 0]]

In the TilePuzzle class, write a method perform_move(self, direction) that attempts to swap the empty tile with its neighbor in the indicated direction, where valid inputs are limited to the strings “up”, “down”, “left”, and “right”. If the given direction is invalid, or if the move cannot be performed, then no changes to the puzzle should be made. The method should return a Boolean value indicating whether the move was successful.

&gt;&gt;&gt; p = create_tile_puzzle(3, 3)                                                            &gt;&gt;&gt; p = create_tile_puzzle(3, 3)

&gt;&gt;&gt; p.perform_move(“up”)            &gt;&gt;&gt; p.perform_move(“down”) True             False

&gt;&gt;&gt; p.get_board()                                                                                      &gt;&gt;&gt; p.get_board()

[[1, 2, 3], [4, 5, 0], [7, 8, 6]]                                                                     [[1, 2, 3], [4, 5, 6], [7, 8, 0]]

In the TilePuzzle class, write a method scramble(self, num_moves) which scrambles the puzzle by calling perform_move(self, direction) the indicated number of times, each time with a random direction. This method of scrambling guarantees that the resulting configuration will be solvable, which may not be true if the tiles are randomly permuted. <em>Hint: The </em>random<em> module contains a function </em>random.choice(seq)<em> which returns a random element from its input sequence.</em>

In the TilePuzzle class, write a method is_solved(self) that returns whether the board is in its starting configuration.

In the TilePuzzle class, write a method copy(self) that returns a new TilePuzzle object initialized with a deep copy of the current board. Changes made to the original puzzle should not be reflected in the copy, and vice versa.

In the TilePuzzle class, write a method successors(self) that yields all successors of the puzzle as (direction, new-puzzle) tuples. The second element of each successor should be a new TilePuzzle object whose board is the result of applying the corresponding move to the current board. The successors may be generated in whichever order is most convenient, as long as successors corresponding to unsuccessful moves are not included in the output.

&gt;&gt;&gt; p = create_tile_puzzle(3, 3)                                                                 &gt;&gt;&gt; b = [[1,2,3], [4,0,5], [6,7,8]]

&gt;&gt;&gt; for move, new_p in p.successors():                                        &gt;&gt;&gt; p = TilePuzzle(b)

…     print move, new_p.get_board() &gt;&gt;&gt; for move, new_p in p.successors(): …           …     print move, new_p.get_board() up   [[1, 2, 3], [4, 5, 0], [7, 8, 6]] …

left [[1, 2, 3], [4, 5, 6], [7, 0, 8]]     up    [[1, 0, 3], [4, 2, 5], [6, 7, 8]] down  [[1, 2, 3], [4, 7, 5], [6, 0, 8]] left  [[1, 2, 3], [0, 4, 5], [6, 7, 8]] right [[1, 2, 3], [4, 5, 0], [6, 7, 8]]

<ol start="3">

 <li><strong>[20 points] </strong>In the TilePuzzle class, write a method find_solutions_iddfs(self) that yields all optimal solutions to the current board, represented as lists of moves. Valid moves include the four strings “up”, “down”, “left”, and “right”, where each move indicates a single swap of the empty tile with its neighbor in the indicated direction. Your solver should be implemented using an iterative deepening depth-first search, consisting of a series of depth-first searches limited at first to <em>0</em> moves, then <em>1</em> move, then <em>2</em> moves, and so on. You may assume that the board is solvable. The order in which the solutions are produced is unimportant, as long as all optimal solutions are present in the output.</li>

</ol>

<em>Hint: This method is most easily implemented using recursion. First define a recursive helper method </em>iddfs_helper(self, limit, moves)<em> that yields all solutions to the current board of length no more than </em>limit<em> which are continuations of the provided move list. Your main method will then call this helper function in a loop, increasing the depth limit by one at each iteration, until one or more solutions have been found.</em>

&gt;&gt;&gt; b = [[4,1,2], [0,5,3], [7,8,6]]                                                             &gt;&gt;&gt; b = [[1,2,3], [4,0,8], [7,6,5]]

&gt;&gt;&gt; p = TilePuzzle(b)                                                                                 &gt;&gt;&gt; p = TilePuzzle(b)

&gt;&gt;&gt; solutions = p.find_solutions_iddfs()      &gt;&gt;&gt; list(p.find_solutions_iddfs()) &gt;&gt;&gt; next(solutions)              [[‘down’, ‘right’, ‘up’, ‘left’, ‘down’, [‘up’, ‘right’, ‘right’, ‘down’, ‘down’]              ‘right’], [‘right’, ‘down’, ‘left’,  ‘up’, ‘right’, ‘down’]]

<ol start="4">

 <li><strong>[20 points] </strong>In the TilePuzzle class, write a method find_solution_a_star(self) that returns an optimal solution to the current board, represented as a list of direction strings. If multiple optimal solutions exist, any of them may be returned. Your solver should be implemented as an A* search using the Manhattan distance heuristic, which is reviewed below. You may assume that the board is solvable. During your search, you should take care not to add positions to the queue that have already been visited. It is recommended that you use the PriorityQueue class from the Queue module.</li>

</ol>

Recall that the Manhattan distance between two locations <em>(r_1, c_1)</em> and <em>(r_2, c_2)</em> on a board is defined to be the sum of the componentwise distances: <em>|r_1 – r_2| + |c_1 – c_2|</em>. The Manhattan distance heuristic for an entire puzzle is then the sum of the Manhattan distances between each tile and its solved location.

&gt;&gt;&gt; b = [[4,1,2], [0,5,3], [7,8,6]]                                                             &gt;&gt;&gt; b = [[1,2,3], [4,0,5], [6,7,8]]

&gt;&gt;&gt; p = TilePuzzle(b)                                                                                 &gt;&gt;&gt; p = TilePuzzle(b)

&gt;&gt;&gt; p.find_solution_a_star()                                                                  &gt;&gt;&gt; p.find_solution_a_star()

[‘up’, ‘right’, ‘right’, ‘down’, ‘down’]                                                        [‘right’, ‘down’, ‘left’, ‘left’, ‘up’,

‘right’, ‘down’, ‘right’, ‘up’, ‘left’,

‘left’, ‘down’, ‘right’, ‘right’]

If you implemented the suggested infrastructure described in this section, you can play with an interactive version of the Tile Puzzle using the provided GUI by running the following command:

The arguments rows and cols are positive integers designating the size of the puzzle.

In the GUI, you can use the arrow keys to perform moves on the puzzle, and can use the side menu to scramble or solve the puzzle. The GUI is merely a wrapper around your implementations of the relevant functions, and may therefore serve as a useful visual tool for debugging.

<h1>2.   Grid Navigation [15 points]</h1>

In this section, you will investigate the problem of navigation on a two-dimensional grid with obstacles. The goal is to produce the shortest path between a provided pair of points, taking care to maneuver around the obstacles as needed. Path length is measured in Euclidean distance. Valid directions of movement include up, down, left, right, up-left, up-right, down-left, and down-right.

Your task is to write a function find_path(start, goal, scene) which returns the shortest path from the start point to the goal point that avoids traveling through the obstacles in the grid. For this problem, points will be represented as two-element tuples of the form (row, column), and scenes will be represented as two-dimensional lists of Boolean values, with False values corresponding empty spaces and True values corresponding to obstacles. Your output should be the list of points in the path, and should explicitly include both the start point and the goal point. Your implementation should consist of an A* search using the straight-line Euclidean distance heuristic. If multiple optimal solutions exist, any of them may be returned. If no optimal solutions exist, or if the start point or goal point lies on an obstacle, you should return the sentinal value None.

&gt;&gt;&gt; scene = [[False, False, False],                                                                                    &gt;&gt;&gt; scene = [[False, True, False],

…          [False, True , False],                                                                                                    …          [False, True, False],

…          [False, False, False]]                                                                                                    …          [False, True, False]]

&gt;&gt;&gt; find_path((0, 0), (2, 1), scene)                                                                                                 &gt;&gt;&gt; print find_path((0, 0), (0, 2), scene)

[(0, 0), (1, 0), (2, 1)]                                                                                              None

Once you have implemented your solution, you can visualize the paths it produces using the provided GUI by running the following command:

The argument scene_path is a path to a scene file storing the layout of the target grid and obstacles. We use the following format for textual scene representation: “.” characters correspond to empty spaces, and “X” characters correspond to obstacles.

<h1>3.   Linear Disk Movement, Revisited [15 points]</h1>

Recall the Linear Disk Movement section from Homework 2. The starting configuration of this puzzle is a row of <em>L</em> cells, with disks located on cells <em>0</em> through <em>n – 1</em>. The goal is to move the disks to the end of the row using a constrained set of actions. At each step, a disk can only be moved to an adjacent empty cell, or to an empty cell two spaces away, provided another disk is located on the intervening square.

In a variant of the problem, the disks were distinct rather than identical, and the goal state was amended to stipulate that the final order of the disks should be the reverse of their initial order.

Implement an improved version of the solve_distinct_disks(length, n) function from Homework 2 that uses an A* search rather than an uninformed breadth-first search to find an optimal solution. As before, the exact solution produced is not important so long as it is of minimal length. You should devise a heuristic which is admissible but informative enough to yield significant improvements in performance.

<h1>4.   Dominoes Game [25 points]</h1>

In this section, you will develop an AI for a game in which two players take turns placing <em>1 x 2 </em>dominoes on a rectangular grid. One player must always place his dominoes vertically, and the other must always place his dominoes horizontally. The last player who successfully places a domino on the board wins.

As with the Tile Puzzle, an infrastructure that is compatible with the provided GUI has been suggested. However, only the search method will be tested, so you are free to choose a different approach if you find it more convenient to do so.

The representation used for this puzzle is a two-dimensional list of Boolean values, where True corresponds to a filled square and False corresponds to an empty square.

<ol>

 <li><strong>[0 points] </strong>In the DominoesGame class, write an initialization method</li>

</ol>

__init__(self, board) that stores an input board of the form described above for future use. You additionally may wish to store the dimensions of the board as separate internal variables, though this is not required.

<ol start="2">

 <li><strong>[0 points] </strong><em>Suggested infrastructure.</em></li>

</ol>

In the DominoesGame class, write a method get_board(self) that returns the internal representation of the board stored during initialization.

&gt;&gt;&gt; b = [[False, False], [False, False]]                                                 &gt;&gt;&gt; b = [[True, False], [True, False]]

&gt;&gt;&gt; g = DominoesGame(b)                                                                      &gt;&gt;&gt; g = DominoesGame(b)

&gt;&gt;&gt; g.get_board()                                                                                      &gt;&gt;&gt; g.get_board()

[[False, False], [False, False]]                                                                [[True, False], [True, False]]

Write a top-level function create_dominoes_game(rows, cols) that returns a new DominoesGame of the specified dimensions with all squares initialized to the empty state.

&gt;&gt;&gt; g = create_dominoes_game(2, 2)                                                 &gt;&gt;&gt; g = create_dominoes_game(2, 3)

&gt;&gt;&gt; g.get_board()                                                                                      &gt;&gt;&gt; g.get_board()

[[False, False], [False, False]]          [[False, False, False],  [False, False, False]]

In the DominoesGame class, write a method reset(self) which resets all of the internal board’s squares to the empty state.

&gt;&gt;&gt; b = [[False, False], [False, False]]                                                 &gt;&gt;&gt; b = [[True, False], [True, False]]

&gt;&gt;&gt; g = DominoesGame(b)                                                                      &gt;&gt;&gt; g = DominoesGame(b)

&gt;&gt;&gt; g.get_board()                                                                                      &gt;&gt;&gt; g.get_board()

[[False, False], [False, False]]                                                                [[True, False], [True, False]]

&gt;&gt;&gt; g.reset()                                                                                                &gt;&gt;&gt; g.reset()

&gt;&gt;&gt; g.get_board()                                                                                      &gt;&gt;&gt; g.get_board()

[[False, False], [False, False]]                                                                  [[False, False], [False, False]]

In the DominoesGame class, write a method is_legal_move(self, row, col, vertical) that returns a Boolean value indicating whether the given move can be played on the current board. A legal move must place a domino fully within bounds, and may not cover squares which have already been filled.

If the vertical parameter is True, then the current player intends to place a domino on squares (row, col) and (row + 1, col). If the vertical parameter is False, then the current player intends to place a domino on squares (row, col) and (row, col + 1). This convention will be followed throughout the rest of the section.

&gt;&gt;&gt; b = [[False, False], [False, False]]                                                 &gt;&gt;&gt; b = [[True, False], [True, False]]

&gt;&gt;&gt; g = DominoesGame(b)                                                                      &gt;&gt;&gt; g = DominoesGame(b)

&gt;&gt;&gt; g.is_legal_move(0, 0, True)                                                              &gt;&gt;&gt; g.is_legal_move(0, 0, False)

True                                                                                                                False

&gt;&gt;&gt; g.is_legal_move(0, 0, False)   &gt;&gt;&gt; g.is_legal_move(0, 1, True) True           True

&gt;&gt;&gt; g.is_legal_move(1, 1, True)

False

In the DominoesGame class, write a method legal_moves(self, vertical) which yields the legal moves available to the current player as (row, column) tuples. The moves should be generated in row-major order (i.e. iterating through the rows from top to bottom, and within rows from left to right), starting from the top-left corner of the board.

&gt;&gt;&gt; g = create_dominoes_game(3, 3)                                                         &gt;&gt;&gt; b = [[True, False], [True, False]]

&gt;&gt;&gt; list(g.legal_moves(True))                                                           &gt;&gt;&gt; g = DominoesGame(b)

[(0, 0), (0, 1), (0, 2), (1, 0), (1, 1),                                               &gt;&gt;&gt; list(g.legal_moves(True))

(1, 2)]                                                                                                           [(0, 1)]

&gt;&gt;&gt; list(g.legal_moves(False))                                                                &gt;&gt;&gt; list(g.legal_moves(False))

[(0, 0), (0, 1), (1, 0), (1, 1), (2, 0),   []  (2, 1)]

In the DominoesGame class, write a method perform_move(self, row, col, vertical) which fills the squares covered by a domino placed at the given location in the specified




orientation.

&gt;&gt;&gt; g = create_dominoes_game(3, 3)

&gt;&gt;&gt; g.perform_move(0, 1, True)

&gt;&gt;&gt; g.get_board()

[[False, True,  False],

[False, True,  False],

[False, False, False]]

&gt;&gt;&gt; g = create_dominoes_game(3, 3)

&gt;&gt;&gt; g.perform_move(1, 0, False)

&gt;&gt;&gt; g.get_board()

[[False, False, False],

[True,  True,  False],

[False, False, False]]




In the DominoesGame class, write a method game_over(self, vertical) that returns whether the current player is unable to place any dominoes.

In the DominoesGame class, write a method copy(self) that returns a new DominoesGame object initialized with a deep copy of the current board. Changes made to the original puzzle should not be reflected in the copy, and vice versa.

&gt;&gt;&gt; g = create_dominoes_game(4, 4)                                                 &gt;&gt;&gt; g = create_dominoes_game(4, 4)

&gt;&gt;&gt; g2 = g.copy()                                                                                        &gt;&gt;&gt; g2 = g.copy()

&gt;&gt;&gt; g.get_board() == g2.get_board()          &gt;&gt;&gt; g.perform_move(0, 0, True) True          &gt;&gt;&gt; g.get_board() == g2.get_board()

False

In the DominoesGame class, write a method successors(self, vertical) that yields all successors of the puzzle for the current player as (move, new-game) tuples, where moves themselves are (row, column) tuples. The second element of each successor should be a new DominoesGame object whose board is the result of applying the corresponding move to the current board. The successors should be generated in the same order in which moves are produced by the legal_moves(self, vertical) method.

&gt;&gt;&gt; b = [[False, False], [False, False]]                                                 &gt;&gt;&gt; b = [[True, False], [True, False]]

&gt;&gt;&gt; g = DominoesGame(b)                                                                      &gt;&gt;&gt; g = DominoesGame(b)

&gt;&gt;&gt; for m, new_g in g.successors(True):                                                   &gt;&gt;&gt; for m, new_g in g.successors(True):

…     print m, new_g.get_board()  …     print m, new_g.get_board() …              …

(0, 0) [[True, False], [True, False]]                                                       (0, 1) [[True, True], [True, True]]

(0, 1) [[False, True], [False, True]]

<em>Optional.</em>

In the DominoesGame class, write a method get_random_move(self, vertical) which returns a random legal move for the current player as a (row, column) tuple. <em>Hint: The</em>

random<em> module contains a function </em>random.choice(seq)<em> which returns a random element from its input sequence.</em>

<ol start="3">

 <li><strong>[25 points] </strong>In the DominoesGame class, write a method get_best_move(self, vertical, limit) which returns a <em>3</em>-element tuple containing the</li>

</ol>

best move for the current player as a (row, column) tuple, its associated value, and the number of leaf nodes visited during the search. Recall that if the vertical parameter is True, then the current player intends to place a domino on squares (row, col) and

(row + 1, col), and if the vertical parameter is False, then the current player intends to place a domino on squares (row, col) and (row, col + 1). Moves should be explored rowmajor order, described in further detail above, to ensure consistency.

Your search should be a faithful implementation of the alpha-beta search given on page 170 of the course textbook, with the restriction that you should look no further than limit moves into the future. To evaluate a board, you should compute the number of moves available to the current player, then subtract the number of moves available to the opponent.

&gt;&gt;&gt; b = [[False] * 3 for i in range(3)]                                                               &gt;&gt;&gt; b = [[False] * 3 for i in range(3)]

&gt;&gt;&gt; g = DominoesGame(b)                                                                      &gt;&gt;&gt; g = DominoesGame(b)

&gt;&gt;&gt; g.get_best_move(True, 1)      &gt;&gt;&gt; g.perform_move(0, 1, True) ((0, 1), 2, 6)             &gt;&gt;&gt; g.get_best_move(False, 1)

&gt;&gt;&gt; g.get_best_move(True, 2)                                                              ((2, 0), -3, 2)

((0, 1), 3, 10)                                                                                                  &gt;&gt;&gt; g.get_best_move(False, 2)

((2, 0), -2, 5)

If you implemented the suggested infrastructure described in this section, you can play with an interactive version of the dominoes board game using the provided GUI by running the following command:

The arguments rows and cols are positive integers designating the size of the board.

In the GUI, you can click on a square to make a move, press ‘r’ to perform a random move, or press a number between <em>1</em> and <em>9</em> to perform the best move found according to an alpha-beta search with that limit. The GUI is merely a wrapper around your implementations of the relevant functions, and may therefore serve as a useful visual tool for debugging.

<h1>5.   Feedback [5 points]</h1>

<ol>

 <li><strong>[1 point] </strong>Approximately how long did you spend on this assignment?</li>

 <li><strong>[2 points] </strong>Which aspects of this assignment did you find most challenging? Were there any significant stumbling blocks?</li>

 <li><strong>[2 points] </strong>Which aspects of this assignment did you like? Is there anything you would have changed?</li>

</ol>