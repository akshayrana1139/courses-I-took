# reinforcement-couse-david-silver

Policy Gradient..

This lesson tells about methods that deal directly with policies rather than working on the value functions. 

Earlier we used to parametrise the value function and then act &epsilon; greedily on that to get the best actions (policy), but instead we can parametrise the policy function which given a state and a weight "u", will give out the best action.

&pi;<sub>&theta;</sub> (s, a) = P [a | s, &theta;]

There are cases where evaluating a policy function is much easier(less complex) than evaluating a value function.

In case we are dealing with fully observable MDPs, a deterministic policy will do better, but if you move to the partially observable POMDPs or the features that we use limit the view of the entire world, then it is optimal to use the stochastic based policy methods instead of value based methods which just acts greedily. 

Assuming we know the policy &pi;<sub>&theta;</sub>(s,a) is differentiable and we know the gradient &nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a)

Likelihood ratio (multiplying and dividng by the policy)


&nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) = <sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a)   &nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) / <sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) 

&nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) = <sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) <b>  &nabla; log <sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a) </b>

The bold above is a score function. : This is the thing which would come up if you are doing maximum likelihood. This tells you how to adjust your policy in the direction that gets more of something. 

Quoting StackOverflow here:
```
The score might be seen intuitively as a sort of measure of how close the parameter actually is to what the data suggest it might be (or the other way round if you are that way inclined), signed for the direction of the difference. The variance of the score will tend to increase with more data, so the variance is intuitively an indication of the amount of information the data will give you about the parameter.
```


<b>Softmax Policy</b>

We want to have some smoothly parametrized policy that tells us which action to take in our each set of desecrete set of actions. This is an alternative to &epsilon;-greedy. 

Weigh actions using linear combinations of features: &Phi;(s,a)<sup>T</sup> &theta;</sup>

Probabality of action is directly proprotional to the exponentiated value of ( linear combination of features multiplied by the weights).. so u features ur actions of left and right into two features, and which of them scores more highly when we make weighted sum , we can pick the one with hiher probability. 



&pi;<sub>&theta;</sub>(s,a) &prop; e<sup>&Phi;(s,a)<sup>T</sup> &theta;</sup>

The score function is hea feature for the action that we took - avg feature for all the actions we might have taken

&nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a)  = &Phi;(s,a) - &Sigma;<sub>&pi;<sub>&theta;</sub></sub> [ &Phi;(s , ..) ]

 

<b>Gaussian Policy</b>

We can use this in continuous action space. Mean is a linear combination of state features 
&mu;(s) = &Phi;(s)<sup>T</sup> &Theta; 

Variance may be fixed &sigma;<sup>2</sup> can be paramterised.. 
Policy a &prop; N (&mu;(s), &sigma;<sup>2</sup>)

The Score Function is the action taken - the mean action(how much more than usual), multiplied by the feature and spread pver variance. The intuition is  

&nabla;<sub>&theta;</sub> &pi;<sub>&theta;</sub>(s,a)  = (a - &mu;(s)) &Phi;(S) / &sigma;<sup>2</sup>

<b>Policy Gradient</b>

&nabla;<sub>&theta;</sub> J(&theta;) = E<sub>&pi;<sub>&theta;</sub></sub> [ &nabla;<sub>&theta;</sub> log &pi;<sub>&theta;</sub> (s, a) Q<sup>&pi;</sup> (s,a) ]

Using return v<sub>t</sub> as an unibiased sample of Q<sup>&pi;</sup>(s,a)

Monte Carlo Policy Gradient
<pre>
function REINFORCE  
Initialise &theta; arbitrarily  
for each episode { s1; a1; r2; --- ; s<sub>T-1</sub>; a<sub>T-1</sub>; r<sub>T</sub> } &prop; &pi;<sub>&theta;</sub>  
    for t = 1 to T - 1 do  
        &theta; &larr; &theta; + &alpha; &nabla;<sub>&theta;</sub> log &pi;<sub>&theta;</sub> (s<sub>t</sub>, a<sub>t</sub>) v<sub>t</sub>
    end for
end for
return &theta;
end function
</pre>


We use a critic to estimate the action-value function  
Q<sub>w</sub>(s,a) ~ Q<sup>&pi;&theta;</sup> (s,a)  

Critic Updates action-value function parameters w  
Actor Updates policy parameters &theta;, in direction suggested by critic.

Now this becomes an approximate policy gradient  
&nabla;<sub>&theta;</sub> J(&theta;) ~ E<sub>&pi;&theta;</sub> [ &nabla;<sub>&theta;</sub> log &pi;<sub>&theta;</sub> (s, a) Q<sub>w</sub> (s,a) ]  
&nabla;<sub>&theta; = &alpha; &nabla;<sub>&theta;</sub> log &pi;<sub>&theta;</sub> (s, a) Q<sub>w</sub> (s,a)

Using linear value fn approx. Qw(s; a) = (s; a)>w  
Critic Updates w by linear TD(0)  
Actor Updates &theta; by policy gradient  

<pre>
function QAC
Initialise s, &theta;
Sample a ~ &pi;<sub>&theta;</sub>
for each step do
    Sample reward r = R<sup>a</sup><sub>s</sub>, Sample transition s' ~ P<sup>a</sup><sub>s</sub>, Sample action a' ~ &pi;<sub>&theta;</sub>(s', a')
    &delta; = r + &gamma; Q<sub>w</sub>(s', a') - Q<sub>w</sub>(s, a)
    &theta; = &theta; + &alpha; &nabla;<sub>&theta;</sub> log &pi;<sub>&theta;</sub>(s, a) Q<sub>w</sub>(s, a)
    w &larr; w + &beta;&delta;&Phi;(s, a)
    a &larr; a',  s &larr; s'
end for
end function

</pre>

<b>Improving</b> 

Approximating the policy gradient introduces bias and a biased policy gradient may not find the right solution. , so we need to choose the value function approximation carefully 

We can reduce the variance by subtracting the balance function B(s) from the policy gradient without changing the expectation, we can substitue B(s) = V<sup>&pi;&theta;</sup>(s)

So we can rewrite the policy gradient using the Advantage function A<sup>&pi;&theta;</sup>(s, a)  
A<sup>&pi;&theta;</sup>(s, a) = Q<sup>&pi;&theta;</sup>(s, a) - V<sup>&pi;&theta;</sup>(s, a)   
&nabla; J(&theta;) = E<sub>&pi;&theta;</sub> [ &nabla;<sub>&theta;</sub> log &pi;&theta;(s, a) A<sup>&pi;&theta;</sup>(s,a)]

Using two function approximators and two paramaters v and W

V<sub>v</sub>(s) = V<sup>&pi;&theta;</sup>(s)  
Q<sub>w</sub>(s, a) = Q<sup>&pi;&theta;</sup>(s, a)  
A(s, a) = Q<sub>w</sub>(s, a) - V<sub>v</sub>(s) 


<b>Natural Policy Gradient</b> is parametrisation independent
It finds ascent direction that is closest to vanilla gradient, when changing policy by a small, xed amount









