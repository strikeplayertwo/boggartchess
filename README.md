Hi! Boggart Chess is of course a working title.  

I decided to start a dev log to keep track of my progress and keep myself motivated. I am busy with school, but will hopefully be able to post updates relatively frequently.  

Hopefully, there will also be a bunch of versions of me in the future contributing to this dev log and game.

# The Game
Boggart Chess is a game of chess played between two players where on every move, the position randomizes to a new position with the same eval (balance between players.)  

A player wins by getting into a position that the computer recognizes as a checkmate in a certain amount of moves, and then the player must perform a semi-efficient checkmate from there (with the game still randomizing on every position) or else the game resumes.   

In order to prevent games taking significantly longer than normal chess games, sudden death would initiate after 30 moves, where the eval increases by 50% after each move made by a player before the algorithm gives a new position to the other player. This largely favors the player currently in the lead to win. However, if the player in the eval lead makes three substantial mistakes on their journey to win, the other player gets a chance to "steal" after the eval flips, but if that player makes one substantial mistake the game ends in a draw.

![20251126_001711](https://github.com/user-attachments/assets/5290d849-0253-4359-baa8-81a6a1b4f58f)
This is what the game would look like for you on your opponent's turn. On the large board, you would see your move you just played (red) as well as your opponent's best move(s) and the eval loss from your move at the top (-5). Then, you would see your best move(s) (green) and the opponent's best responses if applicable. On the small board, you would see your opponent's current position.

On your turn, you would see your current move on the large board as well as your opponent's previous position and highlighted move on the small board.

# Purpose of the game
While playing Boggart Chess, players would learn positional chess and theory much faster than playing normal chess. The game forces players to play positions from many openings, and when paired with the instant feedback that this variant gives, the potential for growth from Boggart Chess is enormous. Learning happens best when there is instant feedback, and Boggart Chess gives players a chance to study and compete at the same time--it effectively makes analyzing your games seamless and engaging instead of a slow-paced, after-the-fact analysis. This variant both prevents players from getting trapped in cycles of playing similar openings and games and helps them learn quicker while doing so. 

# Generation
