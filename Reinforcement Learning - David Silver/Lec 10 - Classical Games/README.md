# reinforcement-couse-david-silver

<b>Classical Games</b>

Best response &pi;<sup>i</sup>*(&pi;<sup>-i</sup>) is optimal policy against those policies.  
Nash equilibrium is a joint policy for all players such that every player's policy is a best response  
&pi;<sup>i</sup> = &pi;<sup>i</sup>*(&pi;<sup>-i</sup>)

A perfect information or Markov game is fully observed
- Chess
- Checkers
- Othello
- Backgammon
- Go

An imperfect information game is partially observed
- Scrabble
- Poker

<b>MINIMAX</b>  

A minimax value function maximizes player 1 expected return and minimizes player 2 expected return  
v*(s) = max<sub>&pi;1</sub>min<sub>&pi;2</sup> V&pi;(s)

A minimax policy is a joint policy &pi; = <&pi;1, &pi;2> that achieves
the minimax values  
There is a unique minimax value function  
A minimax policy is a Nash equilibrium  
Minimax values can be
found by depth-rst
game-tree search - Pg 17

The Search tree grows exponentially and it is
Impractical to search to the end of the game, so Instead use value function approximator v(s;w) ~ v*(s)  
Use value function to estimate minimax value at leaf nodes

Alpha - Beta search is the most efficient minimax search and it took decades of research.

- MC

- TD(&lambda;)

- Simple TD (TD): First learn value function by TD Learning and then use value function in minimax search(no learning)

- TD Root

- TD Leaf

- Tree Strap

- Simulation-Based Search

Solution Methods for Imperfect Information Games

Self-play reinforcement learning
e.g. Smooth UCT

