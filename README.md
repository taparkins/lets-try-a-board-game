# Board Game Prototype

This is a simple prototype-project with two objectives:

1. Force me to use Elm / Haskell for a real-ish project
2. Iterate on a board game idea I'm playing around with

The core mechanics of the game are below. These are entirely subject to change as the prototype is adjusted over time. These are described now in order to provide a high-level objective for development.

### Board:

The board is a random NxN grid of tiles (exact N tbd). The tiles are labeled with various numbers of colored symbols. The characteristics of these cards will be used by the players to achieve objectives, determined for them privately at the beginning of the game. The characteristics of the tiles will be:

* **Color**: e.g. Red, Blue, Green
* **Shape**: e.g. ■, ▲, ●
* **Count**: Number of symbols on the card

The exact set of characteristics, combiniations of tiles, etc. is TBD. But the layout for the grid will definitely be randomized each game.

### Objective cards:

At the beginning of the game, a player is given three objective cards. There is no definitive list of these cards, but they should have a ton of variety so players will have a hard time deciphering what others' objectives are. Examples:

 * Four tiles in one row
 * Two red-colored tiles
 * One ■, one ▲, and one ● tile
 * Three adjacent tiles

After tiles have been selected in voting (more details below), players may reveal objectives they have fulfilled, and remove selections from the tiles used to achieve that objective. That player receives a point, and draws a new objective card.

### Voting

Every round, players will vote for which tile to place a marker on. Voting takes the following steps:

1. Each player simultaneously votes for a candidate tile
2. Each player simultaneously votes for one of the candidate tiles, but they cannot vote for a tile they voted for in the first phase
3. Finally, each player simutaneously votes for any of the tiles that are currently winning the election

Once all three phases are complete, any tiles with the highest total score are selected. Players then take turns fulfilling objective cards in priority order.

Some example elections: Alice, Bob, Candice, Dirk, and Edwin (A, B, C, D, and E going forward) are playing.

#### Example 1:

* In the first election round, A votes for Tile 1, B votes for Tile 2, C votes for Tile 3, and D and E each vote for Tile 4.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A          | 1     |
| 2    | B          | 1     |
| 3    | C          | 1     |
| 4    | D,E        | 2     |

* In the second election round, A votes for Tile 2, B votes for Tile 4, C votes for Tile 2, D votes for Tile 1, and E votes for Tile 3.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,D        | 2     |
| 2    | B,A,C      | 3     |
| 3    | C,E        | 2     |
| 4    | D,E,B      | 3     |

* In the third election round, tiles 2 and 4 are the only candidates. A votes for Tile 2, B votes for Tile 4, C votes for Tile 2, D votes for Tile 4, and E votes for Tile 2.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,D        | 2     |
| 2    | B,A,C,A,C,E| 6     |
| 3    | C,E        | 2     |
| 4    | D,E,B,B,D  | 5     |

 * Tile 2 is selected

#### Example 2:

* In the first election round, A and B each vote for Tile 1, C votes for Tile 2, and D and E each vote for Tile 3.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,B        | 2     |
| 2    | C          | 1     |
| 3    | D,E        | 2     |

* In the second election round, A votes for Tile 2, B votes for Tile 3, C votes for Tile 1, D votes for Tile 1, and E votes for Tile 2.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,B,C,D    | 4     |
| 2    | C,A,E      | 3     |
| 3    | D,E,B      | 3     |

* In the third election round, everyone is forced to vote for tile 1.

| Tile | Votes             | Score |
|------|-------------------|-------|
| 1    | A,B,C,D,A,B,C,D,E | 9     |
| 2    | C,A,E             | 3     |
| 3    | D,E,B             | 3     |

 * Tile 1 is selected

#### Example 3:

* In the first election round, A votes for Tile 1, B votes for Tile 2 C votes for Tile 3, D votes for Tile 4, and E votes for Tile 5.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A          | 1     |
| 2    | B          | 1     |
| 3    | C          | 1     |
| 4    | D          | 1     |
| 5    | E          | 1     |

* In the second election round, A votes for Tile 2, B votes for Tile 3, C votes for Tile 4, D votes for Tile 5, and E votes for Tile 1.

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,E        | 2     |
| 2    | B,A        | 2     |
| 3    | C,B        | 2     |
| 4    | D,C        | 2     |
| 5    | E,D        | 2     |

* In the third election round, everyone votes for their first pick:

| Tile | Votes      | Score |
|------|------------|-------|
| 1    | A,E,A      | 3     |
| 2    | B,A,B      | 3     |
| 3    | C,B,C      | 3     |
| 4    | D,C,D      | 3     |
| 5    | E,D,E      | 3     |

 * Every tile is selected

### Priority

Fulfilling objectives happens in priority order. Priority is determined by how many votes a player cast on tiles that ended up losing the election. If two players have the same priority, their order is determined at random.

So, using the previous example elections:

#### Example 1

Tile 2 was selected as a winner, So votes for tiles 1, 3, and 4 are counted towards priority. So player priority will be:

| Player | Priority |
|--------|----------|
| A      | 1        |
| B      | 2        |
| C      | 1        |
| D      | 2        |
| E      | 2        |

B, D, and E will have first priority (ordered randomly between themselves) for resolution. A and C (again ordered randomly) will have second priority.

#### Example 2

Tile 1 was selected as a winner, So votes for tiles 1, 3, and 4 are counted towards priority. So player priority will be:

| Player | Priority |
|--------|----------|
| A      | 1        |
| B      | 1        |
| C      | 1        |
| D      | 1        |
| E      | 2        |

E has top priority, and the remaining players will be ordered randomly.

#### Example 3

Each tile was selected, so all priority is 0:

| Player | Priority |
|--------|----------|
| A      | 0        |
| B      | 0        |
| C      | 0        |
| D      | 0        |
| E      | 0        |

Resolution order will be chosen entirely at random.

### Winning

The game ends once one player has 5 points. That player is declared as the winner.
