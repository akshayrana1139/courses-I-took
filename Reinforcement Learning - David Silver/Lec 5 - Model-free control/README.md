# reinforcement-couse-david-silver

We can just replace the MC in the previous Policy iteration methods..

MONTE CARLO CONTROL

Policy Evalulation: Monte Carlo Evalutation

Policy Improvement: Greedy Policy Imporvement

Problems in above approach???

1. We cannot use the exsiting policy evaluation, as it is not model free.. so we need to change that.

&pi;'(s) = argmax R<sub>s</sub><sup>a</sup> + P<sub>s</sub><sub>s'</sub><sup>a</sup> V(s')

Bttr to use the below Q(s,a)

&pi;'(s) = argmax Q(s,a)

2. Greedy Policy can be bad as we are not exploring anymore.. so we use &epsilon; greedy
    
&pi;(a|s) = {&epsilon;/m + 1 - &epsilon;, if a<sup>*</sup> = argmax Q(s,a), &epsilon;/m otherwise }

So we do 1 episodes, update (Q(s,a)) all states visited, and update the policy immediately..

Can we really traverse each state-action pair?

GLIE: Greedy in the limit with infinite exploration.
1. All State action pairs are explored infintely many times.
2. &epsilon; converges on a greedy policy if &epsilon;<sub>k</sub> = 1/k


GLIE MC: 
For each state S<sub>t</sub> and action A<sub>t</sub> in the episode,

N(S<sub>t</sub>, A<sub>t</sub>) &larr; N(S<sub>t</sub>, A<sub>t</sub>) + 1

Q(S<sub>t</sub>, A<sub>t</sub>) &larr; Q(S<sub>t</sub>, A<sub>t</sub>) + 1/N(S<sub>t</sub>, A<sub>t</sub>) * (G<sub>t</sub> - Q(S<sub>t</sub>, A<sub>t</sub>))

Improve policy based on new action-value function..

1.  &epsilon; &larr; 1/k
2.  &pi; &larr; &epsilon;-greedy (Q)

Applying TD in our control methods in place of MC

SARSA Image here - Page 23

Q(S<sub>t</sub>, A<sub>t</sub>) &larr; Q(S<sub>t</sub>, A<sub>t</sub>) + &alpha;(R + &gamma;(Q(S'<sub>t</sub>, A'<sub>t</sub>) - Q(S<sub>t</sub>, A<sub>t</sub>)) 

Policy Evaluation: SARSA

Policy Imporvement: &epsilon;-greedy policy improvement

We can similarily use n-step SARSA and &lambda;-SARSA ( combining all n-step SARSAs) and we can use elgibility traces for credit assignments like we did in TD Learning.  

On Policy: Learn about policy &pi; from experience sampled from &pi;

Off Policy: Learn about policy &pi; from experience sampled from &mu;

here we were talking about ON POlicy SARSA, lets see more about off policy..

<b>Off Policy Learning</b>


Re-use experience generated from old policies &pi;<sub>1</sub>, &pi;<sub>3</sub>, &pi;<sub>3</sub>... &pi;<sub>t-1</sub>
We can also do multiple things with diffrnt policies like learning about optimal policy while following exploratory policy.. 
or learn multiple policies while follwing one policy..

Two mechanisms of this is:

<b>Importance Sampling</b>


1. Importance Sampling for Off-Policy Monte Carlo -- Adds too much variance and practically impossible to use it in real life.  
2. Importance Sampling for Off-Policy DS: -- and we need to do it just once coz we'll bootstrap after this.. so here it makes more sense and adds lil variance..


<b>Q Learning</b>

1. We now consider off-policy learning of action-values Q(s,a)
2. No Importance Sampling is required. 
3. Next action is chosen using Behaviour policy A<sub>t+1</sub> ~ &mu; (-| S<sub>t</sub>)
4. But we consider every alternate successor action A'~ &pi; (-|S<sub>t</sub>)
5. And update Q(s,a) towards value of alternative action..



SARSA
→ On policy learning method , means it uses the same policy to choose the next action A 

Q-Learning
→ Off policy learning method , means, it uses the target policy (greedy) to choose the best next action A` while following the behavior policy (epsilon-greedy)

Q(S<sub>t</sub>, A<sub>t</sub>) &larr; Q(S<sub>t</sub>, A<sub>t</sub>) + &alpha;( R<sub>t+1</sub> +  &gamma;(Q(S<sub>t+1</sub>, A') - Q(S<sub>t</sub>, A<sub>t</sub>))
 
 We now allow both target(greedy) policy
 
 &pi;(S<sub>t+1</sub>) = argmax Q(S<sub>t+1</sub>, a')

 and behavioural policy(&epsilon;-greedy/exploratory) to improve..

Q-learning target then simplifies:









            



