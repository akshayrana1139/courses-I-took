# reinforcement-couse-david-silver

<b>Model Based Learning</b>

Earlier we leant how to learn policy or value directly from experiences, here we will learn how to directly learn the model from the experience and use planning to construct a value function or policy.

<pre>
Model-Free RL
    No model
    Learn value function (and/or policy) from experience
Model-Based RL
    Learn a model from experience
    Plan value function (and/or policy) from model
</pre>

Learning a model directly is easier as it is done by supervision methods, and can reason about any uncertainity in the future. But in this case, we are adding two approximation errors.

A model M is representation of the MDP (S, A, P, R) with a parameter &eta;  
Learning the transition probablities..   
S<sub>t+1</sub> ~ P<sub>&eta;</sub>(S<sub>t+1</sub> | S<sub>t</sub>, A<sub>t</sub>)  
R<sub>t+1</sub> = R<sub>&eta;</sub>(R<sub>t+1</sub> | S<sub>t</sub>, A<sub>t</sub>)  

Estimating model M<sub>&eta;</sub> from experience and it becomes a supervised learning problem.  
S<sub>1</sub>, A<sub>1</sub> &rarr; R<sub>2</sub>, S<sub>2</sub>  
S<sub>2</sub>, A<sub>2</sub> &rarr; R<sub>3</sub>, S<sub>3</sub>    

Learning s, a &rarr; r is a regression problem.  
Learning s, a &rarr; s' is a density estimation problem. ( KL Divergence)

We can use multiple ways to learn a model. 

- Table Lookup Model  (Use AB Example for explaining)  
- Linear Expectation Model  
- Linear Gaussian Model   
- Gaussian Process Model  
- Deep Belief Network Model  

So once you learn a model, we can start the planning i.e. solving the optimal value/policy function.  
Using favourite planning algorithm  
- Value iteration  
- Policy iteration  
- Tree search  

<b>Sample Based Planning</b>  
Using the model to only sample experience from it.   
S<sub>t+1</sub> ~ P<sub>&eta;</sub>(S<sub>t+1</sub> | S<sub>t</sub>, A<sub>t</sub>)  
R<sub>t+1</sub> ~ R<sub>&eta;</sub>(R<sub>t+1</sub> | S<sub>t</sub>, A<sub>t</sub>)  



Apply model-free RL to samples, e.g.:  
- Monte-Carlo control  
- Sarsa  
- Q-learning  

So even when we have seen very few real experiences, but once we build a model out of it, we can sample infinite experiences out of it, so thats the advantage of building the model. - Pg 17


<b>Integrating both the approaches of model based and model free</b>  

Model-Free RL  
- No model
- Learn value function (and/or policy) from real experience  

Model-Based RL (using Sample-Based Planning)  
- Learn a model from real experience
- Plan value function (and/or policy) from simulated experience

<b>Dyna</b>  
- Learn a model from real experience  
- Learn and plan value function (and/or policy) from real and
simulated experience  

.....  
.....  

<b>Simulation - based Search</b>  
They dont expore the entire state space, and selects the next best action by lookahead (using a model of MDP), and build a search tree with current state s at its root.   
So no need to solve the entire MDP, bbut just needs to solve the sub-MDP starting from now. 

Forward search paradigm using sample-based planning  
Simulate episodes of experience from now with the model  
Apply model-free RL to simulated episodes  

Simulate episodes of experience from now with the model.  
{s<sub>t</sub><sup>k</sup>, A<sub>t</sub><sup>k</sup>, R<sub>t+1</sub><sup>k</sup>... S<sub>T</sub><sup>k</sup>}<sub>k=1</sub><sup>K</sup>  
K &rarr; no. of simulations at t time steps.  
And if u apply Monte Carlo control &rarr; Monte Carlo search  
And if u apply SARSA &rarr; TD Search  

<b>Monte Carlo Tree Search</b>  

Given a model M  
Simulate K episodes from current state st using current simulation policy &pi;   
{s<sub>t</sub><sup>k</sup>, A<sub>t</sub><sup>k</sup>, R<sub>t+1</sub><sup>k</sup>... S<sub>T</sub><sup>k</sup>}<sub>k=1</sub><sup>K</sup>    

Build a search tree containing visited states and actions  
Evaluate states Q(s; a) by mean return of episodes from s; a  
Q(s; a) = 1/N(s,a) &Sigma;<sup>K</sup><sub>k=1</sub> &Sigma;<sup>T</sup><sub>u=t</sub> 1 (S<sub>u</sub> ,A<sub>u</sub> = s,a) G<sub>u</sub> &rarr;q<sub>&pi;(s,a)   

After search is finished, select current (real) action with maximum value in search tree  
a<sub>t</sub> = argmax Q(s<sub>t</sub> , a)

In MCTS, the simulation policy &pi; improves  
Each simulation consists of two phases (in-tree, out-of-tree)  
- Tree policy (improves): pick actions to maximise Q(S;A)
- Default policy (xed): pick actions randomly  

Repeat (each simulation)  
- Evaluate states Q(S;A) by Monte-Carlo evaluation
- Improve tree policy, e.g. by  􀀀 greedy(Q)  

Monte-Carlo control applied to simulated experience

<b>Temporal-Difference Search</b>

Using TD instead of MC (bootstrapping)

Simulate episodes from the current (real) state s<sub>t</sub>  
Estimate action-value function Q(s; a)  
For each step of simulation, update action-values by Sarsa  
&Delta;Q(S,A) = &alpha;(R + &gamma;Q(S',A') - Q(S,A)) 

Select actions based on action-values Q(s; a) e.g. &epsilon;-greedy  
May also use function approximation for Q

<b>Dyna -2 </b>  


In Dyna-2, the agent stores two sets of feature weights
- Long-term memory
- Short-term (working) memory

Long-term memory is updated from real experience using TD learning  
General domain knowledge that applies to any episode

Short-term memory is updated from simulated experience using TD search . 
Specic local knowledge about the current situation Over value function is sum of long and short-term memories












