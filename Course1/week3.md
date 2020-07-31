#### Module 4 Learning Objectives

**Lesson 1: Policies and Value Functions**

- Recognize that a policy is a distribution over actions for each possible state

  A policy tells us the probabilty of choosing action $a$ and state $s$. It is notated by $\pi(a|s)$. Therefore, the sum of the probability of all the possible actions at one state is 1. 

- Describe the similarities and differences between stochastic and deterministic policies

  Similarities 

  	* Both try to maximize the expected reward. 

  Differences

  * Deterministic policy tells us which action to take at each state, but stochastic policies give a distribution of actions. 

  

- Identify the characteristics of a well-defined policy

  In a well-defined policy, current action only depends on the current state. We assume that the current state contains all the necessary information to make the best decision. 

- Generate examples of valid policies for a given MDP

  Choosing left or right path with equal probability. 

- Describe the roles of state-value and action-value functions in reinforcement learning

  State-value functions tell us the expected return at a given state and following a specific policy $\pi$.  
  $$
  v_\pi(s) = \mathbb{E}_\pi[G_{t+1} | S_t=s]
  $$
  

  Action-value functions tell us the expected return for a particular state-action pair, following policy $\pi$. 
  $$
  q_\pi(s,a) = \mathbb{E}[G_{t+1}|S_t=s, A_t=a]
  $$
  

- Describe the relationship between value functions and policies

  Value functions are computed by following a specific policy. 

- Create examples of valid value functions for a given MDP

  Chess game. 

**Lesson 2: Bellman Equations**

- Derive the Bellman equation for state-value functions

  Bellman equation expresses the current value function in terms of the future value function. 

  <img src="../assets/Screen Shot 2020-07-31 at 5.09.28 PM.png" width="300px"/>
  $$
  v_\pi(s) = \sum_\pi \pi(a|s) \sum_{s',r}p(s',r|s,a)[r + \gamma v_\pi(s')]
  $$
  

- Derive the Bellman equation for action-value functions
  $$
  q_\pi(s,a) = \sum_{s',r}p(s',r|s,a)[r + \gamma \sum_{a'}\pi(a',s')q_\pi(s',a')]
  $$
  

- Understand how Bellman equations relate current and future values

  Bellman equations are recursive! 

- Use the Bellman equations to compute value functions

**Lesson 3: Optimality (Optimal Policies & Value Functions)**

- Define an optimal policy

  Optimal policy is a policy that maximizes the total reward. It is a policy that is better than all the other possible policies for all the states. It doesn't have to be unique, but it is the best one. 

- Understand how a policy can be at least as good as every other policy in every state

  By choosing the best policy at each state, it is guaranteed that there is an optimal policy. 

  <img src="../assets/Screen Shot 2020-07-31 at 5.24.24 PM.png" width="300px"/>

  Let's say there is a policy $\pi_1$ and $\pi_2$ which does better than the other in some states. We can easily come up with a new policy $\pi_3$ that combines the two at their best states. 

- Identify an optimal policy for given MDPs

- Derive the Bellman optimality equation for state-value functions
  $$
  v_\pi^*(s) = max_\pi (v_\pi(s))
  $$
  

- Derive the Bellman optimality equation for action-value functions
  $$
  q_\pi^*(s,a) = max_\pi(q_\pi(s,a))
  $$
  

- Understand how the Bellman optimality equations relate to the previously introduced Bellman equations

  The value under the optimal policy can be expressed as the expected return for the best action at that state $s$. 
  $$
  v_*(s) = max (q_{\pi^*}(s,a))
  $$

  $$
  = max_a\sum_{s',r'}p(s',r|s,a)[r+\gamma v_*(s')]
  $$

  

- Understand the connection between the optimal value function and optimal policies
  $$
  q_\pi^*(s,a) = \mathbb{E}[R_{t+1} + \gamma v*(S_{t+1}) | S_t=s, A_t=a]
  $$
  

- Verify the optimal value function for given MDPs



#### Exercises from book 

**3.11** If the current state is $S_t$, and actions are selected according to stochastic policy $\pi$ , then what is the expectation of $R_{t+1}$ in terms of $\pi$ and the four-argument function $p$? 

​	The four-argument function represents the probabilty of arriving at a new state $s'$ and receiving reward $r$ , when taking action $a$ from state $s$. 
$$
p(s',r|s,a) = Pr[S_t=s', R_t=r|S_{t-1}=s, A_{t-1}=a]
$$
Then the expectection of $R_{t+1}$ is a weighted sum of the above probability equation by the given policy $\pi$. 
$$
\mathbb{E}[R_{t+1} | S_t = s] = \sum_{a} \pi(a|s)\sum_{r \in R}r \sum_{s' \in S^+} p(s',a|s, a)
$$
**3.12** Give an equation for $v_{\pi}$ in terms of $q_\pi$ and $\pi$. 

​	Value function in terms of policy $\pi$ and action-value function $q_\pi$. 
$$
v_\pi(s) = \sum_a \pi(a|s)q_\pi(s,a)
$$
​	It is the weighted sum of all possible actions x its corresponding action-value. 

**3.13** Give an equation for $q_\pi$ in terms of $v_\pi$ and the four-argument $p$. 

​	Action-value function in terms of value funtion $v_\pi$ and four argument $p$. 
$$
q_\pi(s,a) = \sum_{r,s'}rp(s',r|s,a) + \gamma v_\pi(s)
$$
​	It is the expected reward for taking action $a$ at state $s$, plus the value function for being in that state. 

**3.14** The Bellman equation must hold for each state for the value function $v_\pi$ shown in Figure 3.2 of Example 3.5. Show numerically that this equation holds for the center state, valued at +0.7, with respect to its four neighbouring states, valued at +2.3, +0.4, -0.4, and +0.7. 
$$
v_\pi(s) = \sum_a \pi(a|s) \sum_{s',r}p(s',r|s,a)[r+\gamma v_\pi(s')]
$$
​	From the exercise $\gamma=0.9$ , $\pi(a|s) = 0.25$ ,  $p(s',r|s,a) =1 $. 
$$
v_\pi(s) = 0.25 * [2.3 * 0.9 + 0.4 * 0.9 - 0.4 * 0.9 + 0.7 * 0.9]
$$

$$
= 0.675 \approx 0.7
$$

**3.15** In the gridworld example, rewards are positive for goals, negative for running into the edge of the world, and zero the rest of the time. Are the signs of these rewards important, or only the intervals between them? Prove, using (3.8), that adding a constant $c$ to all the rewards adds a constant, $v_c$, to the values of all states, and thus does not affect the relative values of any states under any policies. What is the $v_c$ in terms of $c$ and $\gamma$? 

​	Equation 3.8 expresses return value at time t as the sum of all the rewards received from time t+1. 
$$
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2R_{t+3} + ... = \sum_{k=0}^\infty \gamma^k R_{t+k+1}	
$$
​	Adding the constant $c$ to all the rewards,
$$
G_t = R_{t+1}+c+\gamma(R_{t+2} + c) + \gamma^2(R_{t+3}+c) + ... = \sum_{k=0}^\infty\gamma^k(R_{t+k+1} + c)
$$

$$
= \sum_{k=0}^\infty\gamma^kR_{t+k+1} + \sum_{k=0}^\infty\gamma^kc
$$

​	So adding a constant will add  $v_c = \sum_{k=0}^\infty\gamma^kc$ from the equation 3.8. 

**3.16** Now consider adding a constant $c$ to all the rewards in an episodic task, such as maze running. Would this have any effect, or would it leave the task unchanged as in the continuing task above? Why or why not? Give an example. 

​	In the case of episodic tasks, adding a constant makes a difference, because for longer episodes, the constant will be added more times than the shorter episodes. 

**3.17** What is the Bellman equation for action values, that is, for $q_\pi$? It must give the action value $q_\pi(s',a')$ in terms of the action values, $q_\pi(s',a')$ , of possible successors to the state-action pair $(s,a)$. 
$$
q_\pi(s,a) = \sum_{s',r}p(s',r|s,a)[r+\gamma v_\pi(s')]
$$

$$
= \sum_{s',r} p(s',r|s,a)[r + \gamma \sum_{a'}\pi(a'|s')q_\pi(s',a')]
$$



**3.18** The value of a state depends on the values of the actions possible in that state and on how likely each action is to be taken under the current policy. We can think of this in terms of a small backup diagram rooted at the state and considering each possible action : 

Give the equation corresponding to this intuition and diagram for the value at the root node, $v_\pi(s)$ in terms of the value at the expected leaf node $q_\pi(s,a)$, given $S_t=s$. This equation should include an expectation conditioned on following the policy $\pi$. Then give a second equation in which the expected value is written out explicity in terms of $\pi(a|s)$ such that no expected value notation appears in the equation. 
$$
v_\pi(s) = \mathbb{E}_a[q_\pi(s,a)]
$$

$$
v_\pi(s) = \sum_a \pi(a|s)q_\pi(s,a)
$$

**3.19** The value of an action, $q_\pi(s,a)$, depends on the expected next reward and the expected sum of the remaining rewards. Again we can think of this in terms of a small backup diagram, this one rooted at an action(state-action pair) and branching to the possible next states:

Give the equation corresponding to this intuition and diagram for the action value, $q_\pi(s,a)$, in terms of the expected next reward $R_{t+1}$,  and the expected next state value, $v_\pi(S_{t+1})$ , given that $S_t=s$ and $A_t=a$. This equation should include an expectation but not one coditioned on following the policy. Then give a second equation, writing out the expected value explicitly in terms of $p(s',r|s,a)$ definined by (3.2), such that no expected value notation appears in the equation. 
$$
q_\pi(s,a) = \mathbb{E}_{s',r}[r_{t+1} + \gamma v_\pi(s')]
$$

$$
q_\pi(s,a) = \sum_{s',r} p(s',r|s,a)[r_{t+1} + \gamma v_\pi(s')]
$$

