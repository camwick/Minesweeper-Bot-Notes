# Summary of Project So Far...

## Current State of `main` branch:

### `Solver.java`

Importing 4 classes:

- Robot
  - mouse manipulation
  - grabs pixel colors off screen
- AWTException
  - used for `Robot` initiation
- Color
  - allows us to interpret the pixel colors
- Toolkit
  - has pretty much everything we need to know about the host computer (screensize most importantly)

Private class fields:

- `Robot bot`
  - Solver object's Robot
- `Board gameBoard`
  - Board object holding the board state
- `int ul_x`
  - x coordinate of the upper left hand corner of the board
- `int ul_y`
  - y coordinate of the upper left hand corner of the board
- `int cellSideLength`
  - Length of of side of an individual cell
- `boolean debug`
  - Boolean responsibe for telling Solver to output debug information
- `int[] startCoord`

  - Integer array holding the x and y coordinate of the starting position (green x on a no-guess game)

  #### Constructors:

  `public Solver(int difficulty)`:

  - Solver constructor that has debugging turned on.

  `public Solver(int difficulty, boolean debug)`:

  - Initializes `Robot` object and creates a `Board` object based on the difficulty passed.

  - Calls 2 methods:
    - `calibrateUpperCorner()`
      - finds the bounds of the game board and cell length
    - `calibrateBoard(true)`
      - `Board` object initializes as an empty board ... this method finds the starting position with `true` as the parameter

#### Methods:

`private void calibrateUpperCorner()`:

The main purpose is to find the upper left hand x and y coordinates of the board on the computer screen. This is acocmplished in 3 steps:

1. Loop starting from the left side of the screen until the first gray bar of the board is found
2. From the first gray bar pixel, loop right until the first white pixel of the baord is found
   - this finds the x coordinate of our game board
3. From the found x coordinate, loop upwards until reaching the first gray pixel pixel of the top border
   - this finds the y coordinate of our game board

With the upper left hand coordinates found, all that is left is to find the length of one of the cell's sides. This is accomplished by looping from the upper left hand corner towards the right until we find a gray pixel of the cell border. We can count every pixel in between the corner and the boarder to find the `cellSideLength`.

If `this.debug = true` (established in the Solver constructor), then the mouse will move along every pixel when finding the x and y coordinates and print out valuable data that may help with debugging.

`private void calibrateBoard(boolean state)`:

The main purpose is to synchronize the state of our `Board` object with the screen's game board. If `true` is passed as a parameter, then we are only wanting to locate the the starting position because at the start of the no-guess mode game everything is unclicked but there is one cell with a green x marking a safe location to start the solving.

If we pass `false`, then we are wanting to update our board object after moves have been made on the screen board. We loop through every cell's middle position and grab the pixel color. Depending on the color of the center pixel, we can determine what number is in the cell. If the pixel is gray, then we need to determine if the cell is unclicked or empty (empty = clicked with no number/mine/flag). There is a border in unclicked cells, so we can check two pixel position colors to tell if the cell is unclicked or just empty.

Debug information will move the mouse to every cell and print out what our board object looks like after looping through all the cells on the screen.

### `Board.java`

Private class fields:

- `final int width`
  - width of the board
  - can't be modified
- `final int height`
  - height of the board
  - can't be modified
- `int mineCount`
  - total mines left in the board
- `Cell[][] board`
  - 2 dimensional arrya of Cell objects
  - The meat and potatoes of the `Board` class

#### Constructors:

`public Board(int width, int height, int mines)`:

Simply loops through the given width and height parameters and initializes `Cell[][]` ~ cells default to unclicked.

#### Get methods

`public getWidth()`:

Returns the board width.

`public int getHeight()`:

Returns the board height.

### `Cell.java`

Private class fields:

- char contents
  - contents of the cell

#### Constructors:

`public Cell()`:

Default constructor that sets `contents` to 'U'.

`public cell(char x)`:

Constructor to create a cell with a specified `char contents`.

#### Methods

`public String toString()`:

Returns the contents of the cell (`this.contents`) as a String.
