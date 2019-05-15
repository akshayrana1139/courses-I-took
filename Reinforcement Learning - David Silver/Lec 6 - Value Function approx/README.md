# reinforcement-couse-david-silver

Till now, we have represented the value function by a lookup table.

Every state S has an entry in V(s), or every action-state pair has an entry Q(s,a)

PROBLEMS WITH LARGE MDP??

1. There are too many actions and states to store in memory. 
2. It is too slow to learn the value of each state.. 

SOLUTIONS

1. So we make use of function approximators.. Nerual networks(function to map state s to v<sub>&pi;</sub>(s))

v'(s, w) ~ v(s) 

q'(s, a, w) ~ q(s,a)

Its not just use to save us some memory, but its useful to generalise over the unseen states. 

We'll update the w-(parameter vector) using MC or TD learning..

s &rarr; [black box] &rarr; v'(s, w)

s,a &rarr; [black box] &rarr; q'(s, a, w)

s &rarr; [black box] &rarr; q'(s, a1, w), q'(s, a2, w), q'(s, a3, w) [Used in atari to output value functions of all actions in one state, to choose wisely what to perform.]

We'll consider differentiable function approximators, e.g. 

1. Linear combinations of features..
2. Neural Network..

J(w) = E<sub>&pi;</sub> [(v<sub>&pi;</sub>(S) - v'(S, w))<sup>2</sup>]

&Delta;w = &alpha; &Sigma;<sub>&pi;</sub> [(v<sub>&pi;</sub>(S) - v'(S, w)) &Delta;<sub>w</sub>v'(S,w) ]

Representing state by a feature vector.. x(S) = [x<sub>1</sub>(S).....  x<sub>n</sub>(S)] , so we can substitute v'(S,w) with this in the above equation.

J(w) = E<sub>&pi;</sub> [( <b>v<sub>&pi;</sub>(S)</b> - x(S)<sup>t</sup>w )<sup>2</sup>] The bold part in the equation is supposed to come from an oracle hich tells us the true value. 

and the gradient wrt w is just x(S)

&Delta;w = &alpha; (v<sub>&pi;</sub>(S) - v'(S, w)) x(S)

Update=step-size X prediction error X feature value

Now since we have used oracle for a true value of v<sub>&pi;</sub>(S) but here we dont have any supervision, but instead we use experience using Monte Carlo or TD Learning to substitute the target.. 


For Monte Carlo: 

&Delta;w = &alpha; &Sigma;<sub>&pi;</sub> [(<b>G<sub>t</sub></b> - v'(S, w)) &Delta;<sub>w</sub>v'(S,w) ]

Return G<sub>t</sub> is an unbiased noisey sample of v<sub>&pi;</sub>( S<sub>t</sub> ), can therefore apply supervised learning. 


For TD(0):

&Delta;w = &alpha; &Sigma;<sub>&pi;</sub> [(<b>R<sub>t+1</sub> + &gamma; v'(S<sub>t+1</sub>, w)</b> - v'(S<sub>t</sub> , w)) &Delta;<sub>w</sub>v'(S,w) ]


The TD target <b>R<sub>t+1</sub> + &gamma; v'(S<sub>t+1</sub>, w)</b> is a biased sample of v<sub>&pi;</sub>( S<sub>t</sub> ), can still apply supervised learning. 


For TD(&lambda;):

&Delta;w = &alpha; &Sigma;<sub>&pi;</sub> [(<b>G<sub>t</sub><sup>&lambda;</sup></b> - v'(S, w)) &Delta;<sub>w</sub>v'(S,w) ]


The &lambda;-return  <b>G<sub>t</sub><sup>&lambda;</sup></b> is a biased sample of v<sub>&pi;</sub>( S<sub>t</sub> ), can still apply supervised learning. 

Steps: 
1. So our parmeterized neural network will define a value function, and act greedily(or some &epsilon; ) with that and evaluate our policy. 
2. So when we run our policy, we can update our weights and get a new value function.
3. Repeat step 1 with the new value function.

We wont waste our million experiences to achieve the function approximator ( coz anyway its not equal to actual q<sub>&pi;</sub>), so we just take some steps towards it, fix the weights and use the latest weights again.

<b>Action Value Function Approximation</b> : Replace the v(S,w) with q(S, A, w) in all above equations. 

&Delta;w = &alpha;(q<sub>&pi;</sub>(S,A) - q'<sub>&pi;</sub>(S,A, w)) x ( S, A )

X(S,A) &rarr; Representing the state and action feature vector. 

<b>Gradient TD</B> &rarr; fixes the problem with TD learning in linear and non linear off policy  and for the control we can use Gradient Q Learning 

Least Squares Prediction

Given a experience D consisting of state, value pairs

D = { (S<sub>1</sub>, v<sub>1</sub><sup>&pi;</sup>), (S<sub>2</sub>, v<sub>2</sub><sup>&pi;</sup>) .... }

Least squares algorithms find parameter vector w minimizing sum-squared error between v'(s<sub>t</sub>, w) and target values v<sub>t</sub><sup>&pi;</sup>

LS(w) = &Sigma;<sup>T</sup><sub>t=1</sub> ( v<sub>t</sub><sup>&pi;</sup> - v'(s<sub>t</sub>, w))<sup>2</sup>


Stochastic Gradient Descent with Experience Replay. 

1. Sample state, value from experience. 

( s, v<sup>&pi;</sup> ) ~ D

2. Apply SGD update

&Delta;w = &alpha; ( v<sup>&pi;</sup> - v'(s,w)) &Delta;<sub>w</sub> v'(s,w)

And this converges to the LS solution above. 


<b>Experience Replay with Deep Q Network</b> -- Off policy Q Learning
1. Experience replay de corelates the trajectory and give you much better stable updates. 
2. Take action a<sub>t</sub> acc to &epsilon;-greedy approach
3. Store transition in memory D (s<sub>t</sub>, a<sub>t</sub>, r<sub>t+1</sub>, s<sub>t+1</sub>)
3. Sample mini batches (64) random samples from the Experience D 
4. Compute Q learning targets w.r.t. old, fixed w parameters. w<sup>-</sup>
5. Optimise MSE btw Q network and Q-targets

L<sub>i</sub>(w<sub>i</sub>) = &Sigma;<sub>s, a, r, s'</sub> ~ D<sub>i</sub> [ r + &gamma; max<sub>a'</sub> Q(s', a'; w<sub>i</sub><sup>-</sup> - Q(s, a; w<sub>i</sub>))  ]

This methods works very well with neural networks compared to SARSA coz of experience replay and the stability it gives during weight updates with randomness..Moreover, we use two Q networks ans we switch around by keeping one w fixed and moving towards other one. 
and after few time steps, we switch around and make our old network equal to our new network.

So we never bootstrap towards the thing that we are updating coz that can be unstable

So old parameter is used to generate our target &rarr;
[ <b>r + &gamma; max<sub>a'</sub> Q(s', a'; w<sub>i</sub><sup>-</sup></b> - Q(s, a; w<sub>i</sub>))  ]

So now the oracle itsef uses old fixed paramters instead of using the fresh new parameters. The reason we fix this is because it gives a more stable update.

The danger with TD learning is that everytime you update ur Q values [ r + &gamma; max<sub>a'</sub> Q(s', a'; w<sub>i</sub><sup>-</sup> - <b>Q(s, a; w<sub>i</sub>)</b>)]
, you are also updating the target values..[ <b>r + &gamma; max<sub>a'</sub> Q(s', a'; w<sub>i</sub><sup>-</sup></b> - Q(s, a; w<sub>i</sub>))  ]

Its called as fitted Q iteration


There are varios methods to jump quickly to Least Squares solution instead of iterating all the way down. 

&Sigma;<sup>T</sup><sub>t=1</sub> ( v<sub>t</sub><sup>&pi;</sup> - v'(s<sub>t</sub>, w))<sup>2</sup> = 0 

&Sigma;<sup>T</sup><sub>t=1</sub> x(s<sub>t</sub>)( v<sub>t</sub><sup>&pi;</sup> - X(S<sub>t</sub>)<sup>T</sup> w = 0 

&Sigma;<sup>T</sup><sub>t=1</sub> x(s<sub>t</sub>) v<sub>t</sub><sup>&pi;</sup> = 
&Sigma;<sup>T</sup><sub>t=1</sub> x(s<sub>t</sub>) X(S<sub>t</sub>)<sup>T</sup> w

w = 
( &Sigma;<sup>T</sup><sub>t=1</sub> x(s<sub>t</sub>) X(S<sub>t</sub>)<sup>T</sup> w ) <sup>-1</sup>  &Sigma;<sup>T</sup><sub>t=1</sub> x(s<sub>t</sub>) v<sub>t</sub><sup>&pi;</sup> 

The direct solution time is usually O(N<sup>3</sup>) or O(N<sup>2</sup>) using Shermann-Morrison 

And again we can plugin G<sub>t</sub>(Monte Carlo) , R<sub>t+1</sub> + &gamma; v'(S<sub>t+1</sub>, w) (TD), or G<sub>t</sub><sup>&lambda;</sup> (TD -Lambda) in replace of v<sup>&pi;</sup>

They have better convergence property than iteration methods. 

<b>Adding an answer from stackoverflow here.. about bootstrapping etc</b>

https://datascience.stackexchange.com/questions/26938/what-exactly-is-bootstrapping-in-reinforcement-learning

```
Bootstrapping in RL can be read as "using one or more estimated values in the update step for the same kind of estimated value".

In most TD update rules, you will see something like this SARSA(0) update:

Q(s,a)←Q(s,a)+α(Rt+1+γQ(s′,a′)−Q(s,a))

The value Rt+1+γQ(s′,a′) is an estimate for the true value of Q(s,a), and also called the TD target. It is a bootstrap method because we are in part using a Q value to update another Q value. There is a small amount of real observed data in the form of Rt+1, the immediate reward for the step, and also in the state transition s→s′.

Contrast with Monte Carlo where the equivalent update rule might be:

Q(s,a)←Q(s,a)+α(Gt−Q(s,a))

Where Gt was the total discounted reward at time t, assuming in this update, that it started in state s, taking action a, then followed the current policy until the end of the episode. Technically, Gt=∑T−t−1k=0γkRt+k+1 where T is the time step for the terminal reward and state. Notably, this target value does not use any existing estimates (from other Q values) at all, it only uses a set of observations (i.e., rewards) from the environment. As such, it is guaranteed to be unbiased estimate of the true value of Q(s,a), as it is technically a sample of Q(s,a).

The main disadvantage of bootstrapping is that it is biased towards whatever your starting values of Q(s′,a′) (or V(s′)) are. Those are are most likely wrong, and the update system can be unstable as a whole because of too much self-reference and not enough real data - this is a problem with off-policy learning (e.g. Q-learning) using neural networks.

Without bootstrapping, using longer trajectories, there is often high variance instead, which, in practice, means you need more samples before the estimates converge. So, despite the problems with bootstrapping, if it can be made to work, it may learn significantly faster, and is often preferred over Monte Carlo approaches.
```

















