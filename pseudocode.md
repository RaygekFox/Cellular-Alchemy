Here will be the pseudocode for the project - a game about using cellular automata to do alchemy.

We will write it in a single HTML file with CSS and JavaScript. 

#About the game:
The worls is infinite, consisting of a grid of chunks, each 32 by 32 cells.
We will use canvas to render the world.
Each chunk will operate under a different set of rules, similar to the rules of Conway's Game of Life.

There will be different kinds of cells, potentially with different properties. 
Each chunk can contain up to 3 different types of cells.

Chunks will also contain stones, which can be collected when a cell with this stone becomes alive.
Stones are used to affect rules of chunks.
Stones can be of the same types as cells.

Each chunk has its own board.
Hierarchy of the board: Board -> Row -> Section -> Circle -> Stones
Board consists of 3 rows, each responsible for a different type of cells in the chunk.
A row has 3 sections:
- First section has one circle in it, containing a stone, defining the type of cells affected by this row.
- Each of the other 2 sections have 9 circles in them. Each circle can contain a number of stones, from 1 to 8. 
- The second section(B) is responsible for the Birth of cells of this type in this chunk. 
- The third section(S) is responsible for the Survival of cells of this type in this chunk.

Types of cells and stones:
1. Yellow
2. Green
3. Blue
4. Red
5. Purple
6. Orange

#Pseudocode:

// Basic outline of an HTML file
Top menu:
- Next generation
- Autoplay
- Speed slider
- Counts of stones in the bag
- Menu button
    - New game
    - Save
    - Load

Canvas with the world. Must be zoomable and draggale in the obvious way. Zoom should be done by mouse wheel. Zoom on the rest of the page should be disabled.

// Global variables
isAutoplay = false;
autoPlayInterval = 1000;
activeChunk = [0,0];

// Data structures

bagOfStones = [];

chunk = [
    coord: [x,y], //Coordinates of the chunk, not cells
    isAlive: false, //Becomes alive when board is not empty(has at least one stone)
    {
        board:
        {row: [
            {
                type: 1, //Type of cells affected by this row
                B: [[1,1,1]], //Birth rules. In this case a cell is born if it has 3 neighbors of type 1.
                S: [[1,1],[1,1,1]] //Survival rules. In this case a cell survives if it has 2 or 3 neighbors of type 1.
            },
            {
                type: 2, //Type of cells affected by this row
                B: [[1,1,1]], //Birth rules. In this case a cell is born if it has 3 neighbors of type 1.
                S: [[1,1],[1,1,1]] //Survival rules. In this case a cell survives if it has 2 or 3 neighbors of type 1.
            },
            {
                type: 3, //Type of cells affected by this row
                B: [[1,1,1]], //Birth rules. In this case a cell is born if it has 3 neighbors of type 1.
                S: [[1,1],[1,1,1]] //Survival rules. In this case a cell survives if it has 2 or 3 neighbors of type 1.
            }
        ]},
    grid: [
        [[0 for i in range(32)] for j in range(32)], //Grid for type 1 cells
        [[0 for i in range(32)] for j in range(32)], //Grid for type 2 cells
        [[0 for i in range(32)] for j in range(32)] //Grid for type 3 cells
    ],
    newGrid: [
        [[0 for i in range(32)] for j in range(32)], //Grid for type 1 cells
        [[0 for i in range(32)] for j in range(32)], //Grid for type 2 cells
        [[0 for i in range(32)] for j in range(32)] //Grid for type 3 cells
    ],
    stones: [
        [1, 20, 14], //for example, stone of type 1 at coordinates [20,14] in this chunk.
        ...nine more arrays like this
    ]
    }
];

// Functions

function nextGeneration() {
    for each chunk
        if chunk.isAlive
            updateGrid(chunk);
            ensureNeighboringChunks(chunk);
            

                    
}

function updateGrid(chunk) {
    for each row in chunk.board
                if row.type != Null
                    for i in range(32)
                        for j in range(32)
                            neighbors = checkNeighbors(chunk, i, j);
                                if chunk.grid[i][j] = 0:
                                    if neighbors is in row.B
                                        chunk.newGrid[i][j] = row.type;
                                else if chunk.grid[i][j] = row.type:
                                    if neighbors is in row.S
                                        chunk.newGrid[i][j] = row.type;
    for each stone in chunk.stones
        if chunk.grid[stone[1]][stone[2]] != 0
            bagOfStones.append(stone[0]);
            stone.remove();
    chunk.grid = chunk.newGrid;
    chunk.newGrid = [[0 for i in range(32)] for j in range(32)]



}

function countNeighbors(chunk, i, j) {
    neighbors = [];
    for each neighbor in chunk.grid //if cell is on the edge, we need to check for neighboring cells in the neighboring chunks.
        neighbors.append(neighbor);
    neighbors.sort();
    return neighbors;
}

function ensureNeighboringChunks(chunk) {
    //for each neighboring coordinates check if a chunk exists in chunks. If not, create it.
}

function createChunk(coord) {
    //add a new chunk to the chunks array. It should have empty board, grid, newGrid, and have 10 random stones of type 1. Later we will add logic for creating stones of different types.
}

function showBoard(chunk) {
    //open an overlay with the board of the chunk.
    // It must have a button to close it. It automatically saves the data when it is closed.
    // It must have 3 rows, each with 3 sections, as described above.
    // Stones must be drawn in the circles, according to data of chunk.board. As in the example above, in B Section there are 3 stones of type 1 in one circle and in S section there are 2 stones of type 1 in one circle and 3 stones of type 1 in another circle.
    // Player must be able to drug stones between cirlces.
    // When a stone is dragged to a circle, it should be added to the circle.
    // Somewhere on the screen there is a list of all stones in the bag and the bag icon. If a stone is dragged to the bag icon, it should be added to the bag.
    // Stones can be dragged from the bag to the circles.
    // If a stone is dragged in the first section, we must check if there is no other row of this type and not allow if there is. 
    // When stones are dragged around, we must update the data of chunk.board and always sort the stones in the circles.

}


//Events:

if there is no data in local storage:
    createChunk([0,0]); //first chunk must have  randomly filled its grid with type 1 cells and 10 stones of type 1 randomly located in spots where there are no other cells.
    Then we must save data to local storage.

if Next generation button is clicked:
    nextGeneration();

if Autoplay button is clicked:
    isAutoplay = !isAutoplay;

if isAutoplay:
    setInterval(nextGeneration, autoPlayInterval);

if chunk is clicked:
    autoplay = false;
    showBoard(chunk);

