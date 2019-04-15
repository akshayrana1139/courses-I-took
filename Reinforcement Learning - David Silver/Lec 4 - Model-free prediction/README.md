# reinforcement-couse-david-silver

Lec 3: Dynammic Programming when the model is known 
Lec 4: Model-free prediction: Estimating the value function of an unknown RDP
Lec 5: Model-free control: Optimizing the value function of an unknown RDP


<b>Model Free Prediction</b>

Monte-Carlo Reinforcement Learning: Run out episodes and look for complete returns seen and then just update your estimates of the mean value towards ur sample return for each episode..

Uses empirical mean instead of expected return

Dont need knowledge for transition/rewards and learns directly from complete episodes. 

Goal: Learn V<sub>&pi;</sub> from episodes of experiences under policy &pi;
[S1 A1 R2]... 

Incremental Mean = 1/k &Sigma;<sup>k</sup><sub>j=1</sub> x<sub>j</sub>


Incremental Mean = &mu;<sub>k-1</sub> + 1/k (x<sub>k</sub> -  &mu;<sub>k-1</sub>)

(x<sub>k</sub> -  &mu;<sub>k-1</sub>): is the error in the prediction(mean) and what actually it came out.. and we are correcting our mean in the direction of the mean.



V(S<sub>t</sub>) = V(S<sub>t</sub>) + 1/N(S<sub>t</sub>) { G<sub>t</sub> - V(S<sub>t</sub>) }


{ G<sub>t</sub> - V(S<sub>t</sub>) }: Error we got from the value we thot its gonna be and the actual return function and we are updating our mean estimate in the same direction...
So we update the mean after every episode..


For non stationary problems, as we dont want to remember old episodes.. so we can forget those by adjsuting the &alpha;.
V(S<sub>t</sub>) = V(S<sub>t</sub>) + &alpha; { G<sub>t</sub> - V(S<sub>t</sub>) }


<b>Temporal Difference Learning</b>: Similar to monte carlo but the difference  is that we can learn from incomplete episodes,.. by taking the partial trajectory and estimating the final return by bootstrapping the remainder of the trajectory.  So we basically update our guess(mean) with a new guess(actual value from partial trajectory)

In MC: Updating value V(S) toward <b>actual</ba> return G<sub>t</sub>

V(S<sub>t</sub>) = V(S<sub>t</sub>) + &alpha; { <b>G<sub>t</sub></b> - V(S<sub>t</sub>) }


In TD: Updating value V(S) toward <b>estimated</b> return G<sub>t</sub>

V(S<sub>t</sub>) = V(S<sub>t</sub>) + &alpha; { <b>R<sub>t+1</sub> + &gamma;Vadd(S<sub>t+1</sub>)</b>- V(S<sub>t</sub>) }


Add an image at page 29, 30, 31, 33 to show the diffrnce bwen DP, MC, TD, ES(Exhaustive Search)

TD(0), TD(1), TD(n), YD(n = infinity) = MC

If u average the values of all the step of TD.. its called as TD(&lambda;)

Eligibility Traces: Credit Assignment problem.
