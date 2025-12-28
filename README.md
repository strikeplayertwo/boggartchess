12/23/25  
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
Pros: Positions can be created very quickly, infinite amount can be generated  
Cons: Positions may not be as accurate to real chess games, which may limit their effectiveness in training.  

#2-- Random move sequencing  
Starts with an opening position, then uses probable/slightly random moves to get a new position.  
CentiMax: Prefers for moves that increase the number of potential captures  
Pros: Infinite amount of positions can be generated, positions are mostly accurate to real positions  
Cons: May take a while to generate positions on the fly.   

#3-- Position gathering  
Takes positions from databases of human games.
Opening: Takes positions that stemmed from the desired opening  
CentiMax: Takes positions from Grandmaster games specifically  
Pros: Very accurate to real positions, quick to generate if stored   
Cons: Limited amount of positions available, if not stored in database may take a long time to generate  

# Average Centipawn Loss Model
In addition, for all three methods an algorithm can give feedback on the position that the method has created by assigning it a predicted average centipawn loss score. If this value is too low, another position will be generated. Here are the indicators that may affect the predicted score:  
#of undefended attacked pieces-0 is generally the best, 1 is generally the worst, anything more gets progressively better  
#of total attacked pieces-higher is better  
#of best moves-I'm not sure for now. Less may generally be better, although 1 clear best move may be worse than 2 or 3 best moves.  
Note: For now, #of best moves can be defined as the best move and the number of moves that result in a difference in eval of 0.3 or less from the best move.  
#of pieces on the board-this may have a small effect.  
 
This algorithm to predict average centipawn loss would greatly benefit from being the result of AI that generates positions, keeping track of the values for the above indicators, and then is rewarded based on the average centipawn loss of players that the position is given to. For now, I'll do what I can to have the methods generate effective positions without the use of AI.

# What's going on right now? 
Efficiency, efficiency, efficiency.  
<img width="1477" height="600" alt="image" src="https://github.com/user-attachments/assets/af4fc3b1-381f-405a-afbb-48723794bdc3" />

This is what I currently have coded. The game currently works in some function, as a player by themself can play moves and they will get worse and worse positions from chess tournament games until they get out of the eval range of the given games that were loaded. Getting it to work was difficult, and now every aspect of generation and sorting positions can be improved.  

What makes this so hard is being able to have evaluated positions ready to go that can be loaded efficiently--wait times are currently 10-30 seconds per move in normal ranges. I may look to make a database full of preloaded positions with numerical eval values that the backend can pull from--it currently evaluates random positions after receiving them and waits until it finds a position with an eval in the dedicated range.  

Additionally, having to deeply evaluate the player's move after they make it is definitely a challenge in terms of wait times.  

Development continues to be very technical for now, and will probably be for a while, but I'm much looking forward to creating immersive visuals and sounds for this.  

I hope you find the project interesting, and I am looking forward to your comments, suggestions and questions.  
Thanks for your time! 

12/26/25  
Update 1  
Arrows are now added to the game, showing an arrow for the opponent's best reponse to the move played and the player's best response to that move all on the smaller board. I am planning on adding more arrows soon.  

12/27/25  
Update 2  
Depth 10 instead of Depth 18 is now used to find a position similar to the previous position. Once a similar position is found, it is then evaluated on Depth 18. Then, after the player plays another move and it is evaluated, the difference between the Depth 18 evaluation of the last position the player played and the Depth 18 evaluation of the position they were then given is ADDED to the Depth 18 evaluation of their move on the current position to find a new position. This increases fairness of the game and drastically reduces wait times.  

Also, I have experimented with more arrows, and I will look to expand on that soon. I am looking to include all of the arrows highlighted in green below.  
<img width="740" height="391" alt="image" src="https://github.com/user-attachments/assets/2e97cb03-7e7b-4a0d-a38e-d0db6eb87546" />  

I rewrote it to make it easier to understand:  
<img width="613" height="261" alt="image" src="https://github.com/user-attachments/assets/7babb31c-166e-4431-9114-5c1087338b5f" />

Here is what it would look like:  
<img width="693" height="924" alt="image" src="https://github.com/user-attachments/assets/27abd752-8420-4dfd-a15d-665c3bd477fa" />

This should have everything needed for a simple understanding and analysis, I am very open to any improvements on these methods.  

12/28/25  
Update 3  
I got all of the arrows implemented, but it's a bit questionable:  
<img width="308" height="315" alt="image" src="https://github.com/user-attachments/assets/4172f4b3-33ee-4950-bf98-468076712aa4" />  

As someone who knows what the colors mean and someone who is an experienced chess player, I can figure out what it is trying to tell me. It is saying that the move I played, Qg6, is suboptimal because It allows Qc4, which would set up a discovered attack on my king that I would need to walk out of, then allowing the move e4. Conversely, if I had played the best move highlighted in green, Qe6, then if White had tried the same Qc4 move, I could have played Qxd5 (that's the blue arrow).  

However, it even takes me a while to understand this, and it would be significantly more challenging to players who are either new to chess OR to the arrow and square colors. It's definitely cool but needs some work. I am planning on adding a green arrow to show the best move, and I may think about adding faint/translucent pieces to communicate things more clearly. A translucent black queen on e6 would definitely be useful, and a translucent white queen on c4 or translucent black queen on e8 may be useful. Also, something I could consider in the future would be having multiple boards showing analysis, like one showing the consequences of the move played (red) and another showing the best move and its potential consequences (green/blue). 

Update 4  
<img width="388" height="312" alt="image" src="https://github.com/user-attachments/assets/1cfc6d7e-3084-486c-8705-85f49651fb89" />  
I added the green arrow and it definitely looks cool and helps with analysis, however I don't think this is going to become any more easily understandable without either less arrows or another board.  
