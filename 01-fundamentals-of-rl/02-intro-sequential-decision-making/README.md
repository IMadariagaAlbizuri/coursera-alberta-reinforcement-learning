# Reinforcement Learning – Chapter 2: Multi-armed Bandits

RL uses **evaluative** feedback (how good the action taken was) instead of **instructive** feedback (what the correct action was). This makes active exploration necessary.

## The k-armed bandit problem

- You repeatedly choose among $k$ actions. After each choice you receive a reward drawn from a **stationary** distribution that depends on the chosen action.
- **Goal:** maximize the expected total reward over some period (e.g. 100 steps).
- **Analogy:** a slot machine with $k$ levers.

The **value** of an action is its expected reward:

$$
q_*(a) = \mathbb{E}\left[R_t \mid A_t = a\right]
$$

where $A_t$ is the action taken at step $t$ and $R_t$ the reward received. Since $q_*(a)$ is unknown, we keep an estimate $Q_t(a) \approx q_*(a)$.

### Exploitation vs. exploration

- **Greedy action:** the one with the highest current estimate. Choosing it is **exploiting**.
- **Nongreedy action:** choosing it is **exploring**, since it improves the estimate of that action.
- Exploiting maximizes the expected reward on the *current* step; exploring may give a greater total reward in the *long run*.
- Which is better depends on the estimates, their uncertainties and the number of remaining steps. **RL needs a balance between both.**

## Action-value methods

**Sample-average** estimate:

$$
Q_t(a) \doteq \frac{\text{sum of rewards when } a \text{ taken prior to } t}{\text{number of times } a \text{ taken prior to } t}
= \frac{\sum_{i=1}^{t-1} R_i \cdot \mathbb{1}_{A_i=a}}{\sum_{i=1}^{t-1} \mathbb{1}_{A_i=a}}
$$

- $\mathbb{1}_{A_i=a}$ is 1 if $A_i = a$ and 0 otherwise.
- If the denominator is 0, $Q_t(a)$ is set to a default value (e.g. 0).
- As the denominator tends to infinity, $Q_t(a) \to q_*(a)$ (law of large numbers).

**Greedy selection:** ties are broken arbitrarily.

$$
A_t \doteq \arg\max_a Q_t(a)
$$

It always exploits and never samples apparently inferior actions.

**$\varepsilon$-greedy:** act greedily most of the time, but with small probability $\varepsilon$ choose uniformly at random among all actions, ignoring the estimates. In the limit every action is sampled infinitely often, so every $Q_t(a)$ converges to $q_*(a)$.

## Incremental implementation

Storing every reward is unnecessary. If $Q_n$ is the estimate after $n-1$ rewards, then after the $n$-th reward $R_n$:

$$
Q_{n+1} = Q_n + \frac{1}{n}\left[R_n - Q_n\right]
$$

This follows a general pattern used throughout RL:

$$
\text{NewEstimate} \leftarrow \text{OldEstimate} + \text{StepSize}\,\big[\text{Target} - \text{OldEstimate}\big]
$$

The term $[\text{Target} - \text{OldEstimate}]$ is the *error*, which is reduced by stepping toward the target. Memory and computation per step are constant.

**Simple bandit algorithm ($\varepsilon$-greedy):**

```text
Initialize Q(a) = 0, N(a) = 0 for all a
Loop:
    A <- argmax_a Q(a)  with prob. 1-ε,  random action with prob. ε
    R <- bandit(A)
    N(A) <- N(A) + 1
    Q(A) <- Q(A) + (1/N(A)) * (R - Q(A))
```

## Nonstationary problems

Sample averages suit stationary problems. When reward probabilities change over time, recent rewards should weigh more. A **constant step size** $\alpha \in (0,1]$ does this:

$$
Q_{n+1} \doteq Q_n + \alpha\left[R_n - Q_n\right]
$$

Unrolling it gives an **exponential recency-weighted average**:

$$
Q_{n+1} = (1-\alpha)^n Q_1 + \sum_{i=1}^{n} \alpha (1-\alpha)^{n-i} R_i
$$

The weight of $R_i$ decays with how many steps ago it was observed ($n-i$).

**Variable step size $\alpha_n(a)$.** Convergence with probability 1 is guaranteed if:

$$
\sum_{n=1}^{\infty} \alpha_n(a) = \infty
\qquad \text{and} \qquad
\sum_{n=1}^{\infty} \alpha_n^2(a) < \infty
$$

- 1st condition: steps are large enough to overcome initial conditions and random fluctuations.
- 2nd condition: steps eventually become small enough to ensure convergence.
- $\alpha_n = 1/n$ (sample average) satisfies both. A constant $\alpha$ fails the 2nd one, which is **desirable** in nonstationary problems: the estimates never fully converge and keep tracking the latest rewards.

## Optimistic initial values

All methods depend to some extent on the initial estimates $Q_1(a)$. With sample averages the bias disappears once every action has been tried; with constant $\alpha$ it decays but is permanent.

Initial values can also encourage exploration: set $Q_1(a)$ **higher than any realistic reward**. Every reward then "disappoints", so the agent tries all actions several times, even when acting purely greedily.

- Effective in stationary problems.
- Not suited to nonstationary ones: the exploration drive is temporary, and if the task changes it cannot help.
- In general, any method that treats the initial conditions in a special way is unlikely to handle the nonstationary case.

## Upper-Confidence-Bound (UCB) action selection

$\varepsilon$-greedy explores indiscriminately. UCB instead picks among nongreedy actions according to their **potential to be optimal**, taking into account how close their estimates are to the maximum and how uncertain they are:

$$
A_t \doteq \arg\max_a \left[ Q_t(a) + c\sqrt{\frac{\ln t}{N_t(a)}} \right]
$$

- $N_t(a)$: times action $a$ was selected before $t$ (if $N_t(a)=0$, $a$ is treated as maximizing).
- $c > 0$: controls the degree of exploration.
- The square-root term measures the **uncertainty** in $Q_t(a)$: it shrinks each time $a$ is chosen and grows slowly while it is not.

UCB often performs well, but is harder than $\varepsilon$-greedy to extend beyond bandits. Difficulties:

1. Nonstationary problems.
2. Large state spaces.