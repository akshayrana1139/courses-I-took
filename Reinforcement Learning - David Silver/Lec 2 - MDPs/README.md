# reinforcement-couse-david-silver

For a markov state s and successors state s' , the state transition probability is ==> Pss' = P[St+1 = s' | St = s]

State Transition Probability from all state s to successor state s' == So we have probablities from each state to every state.


       [P11, P12, P13, ... P1n ]
       [P21, P22, P23, ... P2n ]
P ===  [:  , :  , :  , ... :   ] 
       [Pn1, Pn2, Pn3, ... Pnn ]



MARKOV CHAIN PROCESS: is a tuple (S, P) 
S: is a finite set of states s
P: is a state transition probablity

MARKOV REWARD PROCESS: is a tuple of (S, P, R, gamma)
S: is a finite set of states s
P: is a state transition probablity
R: is a reward function, Rs = E[Rt+1 | St = s]
gamma: is a discount factor = [0,1]


return Gt = total discounted reward from time step t 
Gt = Rt+1 + (gamma)Rt+2 + (gamma)^2 Rt+2 + (gamma)^3 Rt+3....


value function v(S) = gives the long term value of state s 
The state value function v(s) = E[Gt | St = s]

Bellman Eqn: Value function can be decomposed into two parts == Immediate reward + discounted value of successor state 

v(s) = E[Gt | St = s]
v(s) = E[Rt+1  + (gamma)Rt+2 + (gamma)^2 Rt+3 | St = s]
v(s) = E[Rt+1  + (gamma){Rt+2 + (gamma) Rt+3)} | St = s]  // Taking out gamma in common to make it a Gt function
v(s) = E[Rt+1  + (gamma)Gt+1 | St = s]  // Using the equation of Gt above to put here
v(s) = E[Rt+1  + (gamma)v(St+1) | St = s]  // Using the equation of v(s) above to put here 
So it states that it is divided into the (immediate reward) + (value function of the next step)...

Expressing Bellman Equation wid matrices..

v = R + (gamma) P v 

v is a column vector with one entry per state..

[ v(1) ]    [ R(1) ]               [ P11.... P1n ]  [ v(1) ]
[   :  ]  = [  :   ]   + (gamma)   [  :       :  ]  [  :   ]
[ v(n) ]    [ R(n) ]               [ Pn1.... Pnn ]  [ v(n) ]

But how do we find V if on both sides... so take inverse

v = ( 1 - (gamma)P )^-1  R   // Computational complexity is O(n^3)

Solutions to solve this value function is... 
1. Monte Carlo Evaluation
2. Temporal - Difference Learning

---------------------------------------------------------------------------------

MARKOV DECISION PROCESS: is a tuple of (S, A, P, R, gamma)
S: is a finite set of states s
A: finite set of actions
P: is a state transition probablity, P[St+1 = s' | St = s, At = a]
R: is a reward function, Rs = E[Rt+1 | St = s, At = a]
gamma: is a discount factor = [0,1]

Policy Pi(a|s) == P[At = a | St = s]

Instead of the state value function v-pi(s), we can now have action value function... q-pi(s, a)... while following policy pi

Using BELLMAN EXPECTED EQUATION here...

v-pi(s) = E[Rt+1 + (gamma) v-pi(St+1) | St = s]

q-pi(s) = E[Rt+1 + (gamma) q-pi(St+1, At+1) | St = s, At = a]


Looking for optimal state-value function v-star(s) and action-value function q-star(s, a) is the maximum function over all policies. 
So MDP is solved when we have q-star

Optimal Policy: a policy is bttr than other policy is v-pi(s) >= v-pi'(s), 
for any MDP.. there is amnm optimum policy and it achieves the optimal value function and the optimum action value function.

Using BELLMAN OPTIMALITY EQUATION here...

q-star(s, a) = Ra-s  + (gamma) Pa-ss Max q-star(s', a')

but this equation is non linear... and has no closed form solution
Many iterative solutions are: 
1. Value Iteration
2. Policy Iteration
3. Q-Learning
4. SARSA



