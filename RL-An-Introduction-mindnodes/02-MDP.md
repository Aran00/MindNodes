# Almost all RL problems can be formalized MDP

1. Formally describe an env for RL  
2. Env is fully observable  
3. The current state completely characterizes the process  
4. Almost all RL problems can be formalized as MDPs


## Markov Process(Chain)

### Markov Property

$$  
\mathcal{P}_{s s^{\prime}}=\mathbb{P}\left[S_{t+1}=s^{\prime} | S_{t}=s\right]  
$$

### State Transition Matrix

### Episodes

### Tuple

- S: a (finite) set of states

- P: a state transition probability matrix

	$$  
	\mathcal{P}_{s s^{\prime}}=\mathbb{P}\left[S_{t+1}=s^{\prime} | S_{t}=s\right]  
	$$

## Markov Reward Process

### Add *R, r* Compared with MP

### Tuple

- S: Finite set of states

- P: state transition probability matrix

	$$  
	\mathcal{P}_{s s^{\prime}}=\mathbb{P}\left[S_{t+1}=s^{\prime} | S_{t}=s\right]  
	$$

- R: state transition probability matrix

	$$  
	\mathcal{R}_{s}=\mathbb{E}\left[R_{t+1} | S_{t}=s\right]  
	$$

- r: Discount factor

	$$  
	\gamma \in[0,1]  
	$$

### Return: $G_t$

- the total discounted reward from time-step t.

	$$  
	G_{t}=R_{t+1}+\gamma R_{t+2}+\ldots=\sum_{k=0}^{\infty} \gamma^{k} R_{t+k+1}  
	$$

### Discount Reasons

1. Mathematically convenient to discount rewards  
2. Avoids infinite returns in cyclic Markov processes  
3. Uncertainty about the future may not be fully represented  
4. If the reward is financial, immediate rewards may earn more  
interest than delayed rewards  
5. Animal/human behaviour shows preference for immediate reward  
6. It is sometimes possible to use undiscounted Markov reward  
processes (i.e. r = 1), e.g. if all sequences terminate

### Value Function: The expected return starting from state s

$$  
v(s)=\mathbb{E}\left[G_{t} | S_{t}=s\right]  
$$

### Bellman Equation

- Decompose Value Function

	$$  
	\begin{aligned} v(s) &=\mathbb{E}\left[G_{t} | S_{t}=s\right] \\ &=\mathbb{E}\left[R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\ldots | S_{t}=s\right] \\ &=\mathbb{E}\left[R_{t+1}+\gamma\left(R_{t+2}+\gamma R_{t+3}+\ldots\right) | S_{t}=s\right] \\ &=\mathbb{E}\left[R_{t+1}+\gamma G_{t+1} | S_{t}=s\right] \\ &=\mathbb{E}\left[R_{t+1}+\gamma v\left(S_{t+1}\right) | S_{t}=s\right] \end{aligned}  
	$$

	- Immediate Reward

	- discounted value of successor state

- Iterative Equation

	$$  
	v(s)=\mathcal{R}_{s}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}} v\left(s^{\prime}\right)  
	$$

- Matrix Form/Linear Equation

	$$  
	v=\mathcal{R}+\gamma \mathcal{P} v  
	$$

	- For Small MRPs

		-  could be solved directly

		- $O(n^3)$ for n states

	- For large MRPs

		- Dynamic programming

		- Monte-Carlo evaluation

		- Temporal-Difference learning

## Markov Decision Process

### Add *A* compared with MRP

### Tuple

- S: a finite set of states

- A: a finite set of actions

- P: a state transition probability matrix

	$$  
	\mathcal{P}_{s s^{\prime}}^{a}=\mathbb{P}\left[S_{t+1}=s^{\prime} | S_{t}=s, A_{t}=a\right]  
	$$

- R: reward function

	$$  
	\mathcal{R}_{s}^{a}=\mathbb{E}\left[R_{t+1} | S_{t}=s, A_{t}=a\right]  
	$$

- r: discount factor

	$$  
	\gamma \in [0,1]  
	$$

### Policies

- a distribution over actions given states

	$$  
	\pi(a | s)=\mathbb{P}\left[A_{t}=a | S_{t}=s\right]  
	$$

- fully defines the behavior of an agent

- Depend on current state(not the history)

- Could be used to transform MDP to

	- MP: State sequence - S1, S2...

		$$  
		\mathcal{P}_{s, s^{\prime}}^{\pi}=\sum_{a \in \mathcal{A}} \pi(a | s) \mathcal{P}_{s s^{\prime}}^{a}  
		$$

	- MRP: State and Reward sequence - S1, R2, S2, ...

		$$  
		\mathcal{R}_{s}^{\pi}=\sum_{a \in \mathcal{A}} \pi(a | s) \mathcal{R}_{s}^{a}  
		$$

### Value Function

- State-value function

	- the expected return starting from state s, and then following policy $\pi$

		$$  
		v_{\pi}(s)=\mathbb{E}_{\pi}\left[G_{t} | S_{t}=s\right]  
		$$

- Action-value function

	- the expected return starting from state s, taking action a, and then following policy $\pi$ 

		$$  
		q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[G_{t} | S_{t}=s, A_{t}=a\right]  
		$$

### Bellman Equation

- Only v or q, with Expectation

	- v iteration

		$$  
		v_{\pi}(s)=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma v_{\pi}\left(S_{t+1}\right) | S_{t}=s\right]  
		$$

	- q iteration

		$$  
		q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[R_{t+1}+\gamma q_{\pi}\left(S_{t+1}, A_{t+1}\right) | S_{t}=s, A_{t}=a\right]  
		$$

- The relation of v and q

	- v from q

		$$  
		v_{\pi}(s)=\sum_{a \in \mathcal{A}} \pi(a | s) q_{\pi}(s, a)  
		$$

		- v from q from v’

			$$  
			v_{\pi}(s)=\sum_{a \in \mathcal{A}} \pi(a | s)\left(\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{\pi}\left(s^{\prime}\right)\right)  
			$$

			- Matrix Form

				$$  
				v_{\pi}=\mathcal{R}^{\pi}+\gamma \mathcal{P}^{\pi} v_{\pi}  
				$$

	- q from v

		$$  
		q_{\pi}(s, a)=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{\pi}\left(s^{\prime}\right)  
		$$

		- q from v from q’

			$$  
			q_{\pi}(s, a)=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in S} \mathcal{P}_{s s^{\prime}}^{a} \sum_{a^{\prime} \in \mathcal{A}} \pi\left(a^{\prime} | s^{\prime}\right) q_{\pi}\left(s^{\prime}, a^{\prime}\right)  
			$$

- Solving: Equal to MRP methods

## MDP: Optimal Problems

### Optimal Value Function

- the maximum value function over all policies

	- v

		$$  
		v_{*}(s)=\max _{\pi} v_{\pi}(s)  
		$$

	- q

		$$  
		q_{*}(s, a)=\max _{\pi} q_{\pi}(s, a)  
		$$

- specifies the best possible performance in the MDP

- When is known, the MDP is “solved”

### Optimal Policy

- A partial order over all policies

	$$  
	\pi \geq \pi^{\prime} \text { if } v_{\pi}(s) \geq v_{\pi^{\prime}}(s), \forall s  
	$$

- Optimal Policy Theorem - For a MDP:

	- There exists an optimal policy $\pi_{*}$ that is better than or equal to all other policies

		$$  
		\pi_{*} \geq \pi, \forall \pi  
		$$

	- All optimal policies achieve the optimal value function

		$$  
		V_{\pi_{*}}(s)=V_{*}(s)  
		$$

	- All optimal policies achieve the optimal action value function

		$$  
		q_{\pi_{*}}(s, a)=q_{*}(s, a)  
		$$

- Finding an Optimal Policy

	- optimal policy can be found by maximizing over q

		$$  
		\pi_{*}(a | s)=\left\{\begin{array}{ll}{1} & {\text { if } a=\underset{a \in \mathcal{A}}{\operatorname{argmax}} q_{*}(s, a)} \\ {0} & {\text { otherwise }}\end{array}\right.  
		$$

		- There is always a deterministic optimal policy for any MDP

		- If we know $q_{*}(s, a)$, we immediately have the optimal policy

### Bellman Equation

- $v_{*}$ from $q_{*}$

	$$  
	V_{*}(s)=\max _{a} q_{*}(s, a)  
	$$

	- $v_{*}$ from $q_{*}$ from $v_{*}$

		$$  
		v_{*}(s)=\max _{a} \mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{*}\left(s^{\prime}\right)  
		$$

- $q_{*}$ from $v_{*}$

	$$  
	q_{*}(s, a)=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} v_{*}\left(s^{\prime}\right)  
	$$

	- $q_{*}$ from $v_{*}$ from $q_{*}$

		$$  
		q_{*}(s, a)=\mathcal{R}_{s}^{a}+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}}^{a} \max _{a^{\prime}} q_{*}\left(s^{\prime}, a^{\prime}\right)  
		$$

- Solving

	- Different with the former ones, not linear, no closed form solution

	- Iterative methods

		- Value iteration

		- Policy iteration

		- Q-learning

		- Sarsa

## MDP Extensions

### Infinite and continuous MDPs

### Partially observable MDPs

### Undiscounted, average reward MDPs

## Backup Diagram

### Composed of S node and A node

### Usually tree shape

### Example

## Key points

### The components(tuple) of MDP

- Value Function

- Policy

### The relationship between MP, MRP and MDP

### Bellman Equation, all kinds 

- For v and q

- Normal and Optimal

	- Normal: Linear

	- Optimal: non-Linear

## Questions

### What is the relationship between it and Markov Stationary Distribution?

- Seems no way to connect

- But still needs more think

### The programming example?

