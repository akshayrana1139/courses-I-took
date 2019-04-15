# reinforcement-couse-david-silver

For Dynamic Programming, we need
1. Optimal Substructure: Optimal solution can be decomposed into subproblems
2. Overlapping Subproblems: Subproblems recur many times and can be cached and reused

MDPs satisfy both the properties as Bellman Eqn breaks down the optimal value function into pieces.. [ Optimal behaviour of immediate step + value fn of next steps ], We can store and reuse the value functions.. 

DP assumes full knowledge of the MDP and is used for planning..

Prediction:
Input: MDP (S, A, P, R, gamma) and policy pi
Output: Value function .. v-pi

Control: (best thing to do in MDP.. full optimization).. So out of all the policies, what is the best policy that will give the best value function..
Input: MDP (S, A, P, R, gamma)
OUtput: Optimal value function .. v-star and the optimal policy.. pi-star


We'll sweep all the states in one iteration and then update the entire v vector i.e. the value function of all states together.. called Synchronous backups
1. At each iteration K+1
2. For all states s = S
3. Update V(k+1) from vk(s') // s' is a successor state of state s
