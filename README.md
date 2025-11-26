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

Boggart Chess is also more effective for learning than traditional chess training such as completing puzzles or tactics because those methods are often restricted to presenting the player positions where there is only one best move, limiting their effectiveness in opening and middlegame play. Boggart Chess presenting the player with positions in which there are multiple best moves shifts more of a focus onto positional play, the most important aspect of long-term learning in chess. In addition, when a player is doing chess puzzles, they know there is only one solution, causing them to look at the board differently and worsening their learning as they already know something important about the position.  

I also have ideas for a helpful review mode, although more info on that will come later.

# Generation
The entire challenge with coding this game is figuring out how to most accurately generate positions. There are three methods I have come up with for this. Every position in one game of Boggart Chess stems from one opening, and I will describe how this is accomplished with each method. Additionally, the goal with this generation is to predict the highest value of average centipawn loss--the average amount a player would mess up the position. We'll call it CentiMax for short, and each method has ways to increase this.

Methods:  
#1-- Random piece placement  
Places pieces somewhat randomly on the board.  
Opening: %of pieces in their opening position is required  
CentiMax: %chance to place kings on opposite side of the board, %chance to generate a piece in the line of sight of another piece (it would place a piece a knight's move away from a knight, on a diagonal from a bishop, etc. as creating the potential for captures may increase CentiMax)  
To increase the accuracy of these positions to real chess positions, kings can be placed near their starting squares or castling squares, no two bishops of the same color can be on the same color square, knights and bishops can be placed in common positions for them, and common pawn formations for the opening can be generated in chunks.  

#2-- Random move sequencing  
Starts with an opening position, then uses probable/slightly random moves to get a new position.  
CentiMax: Prefers for moves that increase the number of potential captures  

#3-- Position gathering  
Takes positions from databases of human games.
Opening: Takes positions that stemmed from the desired opening  
CentiMax: Takes positions from Grandmaster games specifically  

# Average Centipawn Loss Model
In addition, for all three methods an algorithm can give feedback on the position that the method has created by assigning it a predicted average centipawn loss score. If this value is too low, another position will be generated. Here are the indicators that may affect the predicted score:  
#of undefended attacked pieces-0 is generally the best, 1 is generally the worst, anything more gets progressively better  
#of total attacked pieces-higher is better  
#of best moves-I'm not sure for now. Less may generally be better, although 1 clear best move may be worse than 2 or 3 best moves.  
Note: For now, #of best moves can be defined as the best move and the number of moves that result in a difference in eval of 0.3 or less from the best move.  
#of pieces on the board-this may have a small effect.  
 
This algorithm to predict average centipawn loss would greatly benefit from being the result of AI that generates positions, keeping track of the values for the above indicators, and then is rewarded based on the average centipawn loss of players that the position is given to. For now, I'll do what I can to have the methods generate effective positions without the use of AI.

# What's going on right now?  
<img width="945" height="568" alt="image" src="https://github.com/user-attachments/assets/f9283534-2abb-4efc-846a-4741eaca9e04" />
This is what I currently have coded (
