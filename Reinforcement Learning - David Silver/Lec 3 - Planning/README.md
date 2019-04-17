# reinforcement-couse-david-silver

For Dynamic Programming, we need
1. Optimal Substructure: Optimal solution can be decomposed into subproblems
2. Overlapping Subproblems: Subproblems recur many times and can be cached and reused

MDPs satisfy both the properties as Bellman Eqn breaks down the optimal value function into pieces.. [ Optimal behaviour of immediate step + value fn of next steps ], We can store and reuse the value functions.. 

DP assumes full knowledge of the MDP and is used for planning.

Prediction:
Input: MDP (S, A, P, R, &gamma;) and policy pi
Output: Value function .. v-pi

Control: (best thing to do in MDP.. full optimization).. So out of all the policies, what is the best policy that will give the best value function..
Input: MDP (S, A, P, R, &gamma;)
OUtput: Optimal value function .. v-star and the optimal policy.. pi-star

<b>Iterative Policy Evaluation</b>:


We'll sweep all the states in one iteration and then update the entire v vector i.e. the value function of all states together.. called Synchronous backups
1. At each iteration K+1
2. For all states s = S
3. Update V(k+1) from vk(s') // s' is a successor state of state s

We gonna put values from last iteration and compute one single new value for next step.

v<sup>k+1</sup>  = R<sup>&pi;</sup> + &&gamma;; P<sup>&pi;</sup> v<sup>k</sup>  

Example: Small Grid World
Using a  random value function and using that to evaluate the optimal value funtion..


<b>Policy Iteration</b>: process which always converges to  &pi;<sub>*</sub>

Evaluate a policy &pi;

v<sub>&pi;</sub>(s) = E[R<sub>t+1</sub> + &&gamma;;R<sub>t+2</sub> + ... | S<sub>t</sub> = s]

Improve the policy by acting greedly with respect to v<sub>&pi;</sub>

Feedback: Use the new policy to compute value functions instead of computing only on old policies

Even though we are evaluating just one policy, but the value functions we get while evaluating the policy helps us build another better policy...


&pi;' = greedy(v<sub>&pi;</sub>)

If we act greedily, it atleast improves the values by one step.. 

Should we stop at some iteration level as moving in the loop would give us no improvement over the policy as we have already achieved the best optima policy?? -> Modified Policy Iteration

Stopping Early? -- when sm convergence value function achieved 
Stopping at k iteration -- may or may not give optimal bt definitely converge
Why not update policy every iteration?..i.e. stop after k = 1 == [Value Iteration]

Intuition: -> working backward from the final state..

<b>Value Iteration</b>: Finding optimal policy
Iterative application is the same.. 
Syncronous backups--
1. At each iteration K+1
2. For all states s = S
3. Update V<sub>k+1</sub>(s) from v<sub>k+1</sub>(s') // s' is a successor state of state s

Here, we are not building the policy explicitly.. and we go directly from value fn to value fn.. unlike the policy iteration where we find the value func and the policy.

The intermediate value fns in the process of finding an optimal value function may not have a corresponding policy ... unlike in policy iteration whr every value function had a policy for it. 


Asynchronous Dynammic Programming:
1. In place DP
2. Prioritized sweeping
3. Real time DP 

--- fixed by sampling the branches instead of solving all the branches... 
so we dont need to know the dynamics in a model free world as we are anyway samping the choices.


