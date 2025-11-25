Hi! Boggart Chess is of course a working title.
I decided to start a dev log to keep track of my progress and keep myself motivated. I am busy with school, but will hopefully be able to post updates relatively frequently.
Hopefully, there will also be a bunch of versions of me in the future contributing to this dev log and game.

# The game
Boggart Chess is a game of chess played between two players where on every move, the position randomizes to a new position with the same eval (balance between players.)
A player wins by getting into a position that the computer recognizes as a checkmate in a certain amount of moves, and then the player must perform a semi-efficient checkmate from there (with the game still randomizing on every position) or else the game resumes. 
In order to prevent games taking significantly longer than normal chess games, sudden death would initiate after 30 moves, where the eval increases by 50% after each move made by a player before the algorithm gives a new position to the other player. This largely favors the player currently in the lead to win. However, if the player in the eval lead makes three substantial mistakes on their journey to win, the other player gets a chance to "steal" after the eval flips, but if that player makes one substantial mistake the game ends in a draw.

# Generation
