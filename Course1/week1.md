**Difference between supervised learning and RL**

* Evaluative method  vs Instructive method 
  * Evaluative : Action is evaluated by telling how good it was 
  * Instructive : Telling whether the chosen action is the best or the worst one out of all possible actions 

* Simple RL is purely evaluative, but more sophisticated RL algorithms combine evaluative and instructive methods (ex. value function approximation) 

Nonassociative setting : When you only think about acting in only one situation. 



#### Module 2 Learning Objective 

**Lesson 1: The K-Armed Bandit Problem**

- Understand the temporal nature of the bandit problem

  In bandit problem, you are repeatedly make decisions over time. The goal is to maximize the total reward over the given time period. 

   

- Define k-armed bandit

  K-armed bandit is when you have K possible actions that you can take at each decision point. Ex. There are K levers you can pull from a slot machine. 

  

- Define action-values

  The expected reward for taking an action $a$ at time step $t$. 
  $$
  q_*(a) = \mathbb{E}[R_t | A_t = a]
  $$
  It is the "expected" value, because we don't know the exact reward. 

  Ex. When at a slot machine, we don't know the outcome of pulling the lever

- Define reward

  The immediate gain you get from taking an action. This does not consider future rewards. 



**Lesson 2: What to Learn? Estimating Action Values**

- Define action-value estimation methods

  It is a method to estimate the action-value through the observations from the past and to use this estimate to choose actions. 

  The estimation of action-value is done by "sample-average" method. It is simply the average of all the rewards received for when that action was taken in the past.
  $$
  Q_t(a) = \frac{\sum_{i=1}{t-1} R_i * \mathbb{1}_{A_i=a}}{\sum_{i=1}^{t-1} \mathbb{1}_{A_i=a}}
  $$
  

- Define exploration and exploitation

  Exploitation : Always taking greedy actions with the current knowledge of the environment

  Exploration : Being open-minded to non-greedy actions, thinking about longer-term benefit than the immediate rewards. 

- Select actions greedily using an action-value function
  $$
  A_t = argmax_a Q_t(a)
  $$
  

- Define online learning

  Because we need to compute the estimation of action-values at every time step, it is computationally inefficient. There are many redundant computations. We can save time and memory through online learning, where we can keep the computations from the past and just update it. 

- Understand a simple online sample-average action-value estimation method

  Let $Q_{n+1}$ be the estimation of action-value of action $a$ after selecting it $n$ times. 
  $$
  Q_{n+1} = \frac{1}{n} \sum_{i=1}^{n}R_i
  $$

  $$
  = \frac{1}{n}[R_n + \sum_{i=1}^{n-1}R_{n-1}]
  $$

  $$
  = \frac{1}{n}[R_n + (n-1)\frac{1}{n-1}\sum_{i=1}^{n-1}R_{n-1}]
  $$

  $$
  = \frac{1}{n}[R_n + (n-1)Q_n]
  $$

  $$
  = Q_n + \frac{1}{n}[R_n - Q_n]
  $$

  In conclusion, we can see that $Q_{n+1}$ can be written in terms of $Q_n$ that we computed in the previous step and the current reward $R_n$. 

- Define the general online update equation
  $$
  NewEstimate = OldEstimate + StepSize(Target - OldEstimate)
  $$
  where $Target - OldEstimate = error$

- Understand why we might use a constant stepsize in the case of non-stationarity

  Non-stationary case is when the reward probabilties change over time. Since the reward probabilties have been updated (hopefully to a better estimate as experience is gained), we would like to weigh more importance on the recent estimates than the ones from the far past. With a constant stepsize, where stepsize < 1, past rewards get exponentially lower weight.

   

**Lesson 3: Exploration vs. Exploitation Tradeoff**

- Compare the short-term benefits of exploitation and the long-term benefits of exploration

  In case of short-term cases, it is better to exploit, since we don't have time to explore and wait for the better option. However, in case of long-term, there might be an action that brings a really large reward towards the end, so it is better to explore. 

- Understand optimistic initial values

  We need to initialize the action-values in the beginning of training. Instead of setting them to 0, we can set it to a higher value than what is expected. 

- Describe the benefits of optimistic initial values for early exploration

  Taking any action will result in a reward less than the optimistic initial value, in other words, no action is satisfactory, so the agent will not settle with a single action quickly. 

- Explain the criticisms of optimistic initial values

  It only explores in the early stages of the training. Therefore, it is not effective for non-stationary value cases, where the values continuously change in the later stages of training. 

- Describe the upper confidence bound action selection method

  Upper confidence bound action selection is another way of encouraging exploration vs exploitation. Instead of going $\epsilon$-greedy over all possible actions, this narrows down the actions to explore from. It selects the action that is most likely for being optimal. 
  $$
  A_t = argmax_a[Q_t(a) + c \sqrt\frac{ln(t)}{N_t(a)}]
  $$
  It has an added "uncertainty" or "variance" term from the original action selection equation. The term says that if the action $a$ has been seen many times in the past, $N_t(a)$ will be large, so the overall term will be small, thus this action is not encouraged to be selected. 

  In other words, if $a$ is selected, then the uncertainty decreases for $a$. 

- Define optimism in the face of uncertainty



### Exercises for RLBook (Sutton and Barto) Chapter 2 

**2.1** In $\epsilon$-greedy action selection, for the case of two actions and ε = 0.5, what is the probability that the greedy action is selected?

0.5 for taking the greedy action + 0.5 for taking the greedy action when exploring with 0.5 chance. 

0.5 + 0.5 * 0.5 = 0.75 

**2.2** Suppose the initial sequence of actions and rewards is A1 = 1, R1 = -1, A2 = 2, R2 = 1, A3 = 2, R3 = -2, A4 = 2, R4 = 2, A5 = 3, R5 = 0. On some of these time steps the ε case may have ocurred, causing an action to be selected at random. On which time steps did this definitely occur? On which time steps could this possibly have occurred.

|      | a=1  | a=2             | a=3  | a=4  |
| ---- | ---- | --------------- | ---- | ---- |
| t=1  | 0    | 0               | 0    | 0    |
| t=2  | -1   | 0               | 0    | 0    |
| t=3  | -1   | 1               | 0    | 0    |
| t=4  | -1   | (-2+1)/2=-0.5   | 0    | 0    |
| t=5  | -1   | (1-2+2)/2 = 0.5 | 0    | 0    |

$\epsilon$-greedy action is definitely taken at t=4 and 5. 

It could have happened in t=1,2 and/or 3 but we don't know. 

**2.3** In the comparison shown in Figure 2.2, which method will perform the best in the long run in terms of cumulative reward and probability of selecting the best action? How much better will it be? Express your answer quantitatively. 

$\epsilon$=0.01 will be the best in the long run. 

Best action will be taken 99.1% (0.99 + 0.01 * 0.1) while for $\epsilon=0.1$ , it will be taken 91.9% (0.91 + 0.09 * 0.1)

**2.4** If the step size parameters, $\alpha_n$ , are not constant, then the estimate $Q_n$ is a weighted average of previously received rewards with a weighting different from that given by (2.6). What is the weighting on each prior reward for the general case, analogous to (2.6), in terms of the sequence of step size parameters? 

Since it is not constant, it will be a product of the past step sizes. 
$$
Q_{n+1} = Q_n\prod_{i=1}^{n}(1-\alpha_i) + \sum_{i=1}^n \alpha_i R_i\prod_{j=1}^i(1-\alpha_j)
$$
**2.5** Design and conduct an experiment to demonstrate the difficulties that sample-average methods have for nonstationary problems. Use a modified version of the 10-armed testbed in which all the $q_*(a)$ start out equal and then take independent random walks. Prepare plots like figure 2.2 for an action-value method using sample averages, incrementally computed, and another action-value method using a constant step-size parameter, $\alpha=0.1$. Use $\epsilon=0.1$ and longer runs, say of 10,000 steps. 

**2.6** 

**2.7**

**2.8**  (Not sure) UCB will explore a lot (more than $\epsilon$-greedy) in the beginning when the uncertainty is high. As it explores, it will start to discover better actions and therefore the uncertainty for these actions will decrease. 

