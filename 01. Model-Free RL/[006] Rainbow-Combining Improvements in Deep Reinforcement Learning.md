# Rainbow-Combining Improvements in Deep Reinforcement Learning

**Authors**: Matteo Hessel, Joseph Modayil, Hado van Hasselt, Tom Schaul, Georg Ostrovski, Will Dabney, Dan Horgan, Bilal Piot, Mohammad Azar, David Silver

**Year**: 2017

**Links:** [[arxiv](https://arxiv.org/abs/1710.02298)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01.%20Model-Free%20RL/%5B006%5D%20Rainbow-Combining%20Improvements%20in%20Deep%20Reinforcement%20Learning.md)]

**Algorithm**: **Rainbow DQN**

### Highlights

- **Integrated Agent**
- **Double DQN, PER, Dueling DQN**
- **Multi-step Learning**
- **Distributional RL**
- **Noisy Nets**

### Background

- Multiple improvements have been applied to DQN separately. This paper intends to combine all these improvements together to provide SOTA performance on Atari 2600.

#### DRL and DQN

- Refer to [001] Playing Atari with Deep Reinforcement Learning, Mnih et al, 2013. **Algorithm: DQN.** [[arxiv](https://arxiv.org/abs/1312.5602v1)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01.%20Model-Free%20RL/%5B001%5D%20Playing%20Atari%20with%20Deep%20Reinforcement%20Learning.md)]

#### Double Q-Learning

- Refer to [004] Deep Reinforcement Learning with Double Q-learning, Hasselt et al 2015. **Algorithm: Double DQN.** [[arxiv](https://arxiv.org/abs/1509.06461)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01.%20Model-Free%20RL/%5B004%5D%20Deep%20Reinforcement%20Learning%20with%20Double%20Q-learning.md)]

#### Prioritized Experience Replay

- Refer to [005] Prioritized Experience Replay, Schaul et al, 2015. **Algorithm: Prioritized Experience Replay (PER).** [[arxiv](https://arxiv.org/abs/1511.05952)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01.%20Model-Free%20RL/%5B005%5D%20Prioritized%20Experience%20Replay.md)]

#### Dueling DQN

- Refer to [003] Dueling Network Architectures for Deep Reinforcement Learning, Wang et al, 2015. **Algorithm: Dueling DQN.** [[arxiv](https://arxiv.org/abs/1511.06581)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01.%20Model-Free%20RL/%5B003%5D%20Dueling%20Network%20Architectures%20for%20Deep%20Reinforcement%20Learning.md)]

#### Multi-step Learning

- Use multi-step return instead of one-step reward:
  $$
  R_t^{(n)}\equiv\sum_{k=0}^{n-1}\gamma^{(k)}R_{t+k+1}
  $$

- 

$$
(R_t^{(n)}+\gamma_t^{(n)}\max_{a'}q_{\bar\theta}(S_{t+n},a')-q_\theta(S_t,A_t))^2
$$

- The value function can be learned faster in this way, because the value can be more accurately estimated with multi-step return.

#### Distributional RL

- We can learn to approximate the distribution of returns instead of the expected return. The distributional perspective allows us to gain more information on the  risks of the actions. (e.g. {10: 90%, 110: 10%} vs. {15: 50%, 25: 50%})
- We use a discrete distribution to model the value distribution. $[V_{min}, V_{max}]$ is divided into $N_{atoms}$ equal parts as the support $\mathbf{z}$. $d_t=(\mathbf{z},\mathbf{p}_\theta(S_t,A_t))$
- We use KL-Divergence to measure the distance between the distribution $d_t$ and the target distribution $d_t' \equiv (R_{t+1}+\gamma_{t+1}\mathbf{z},\mathbf{p}_{\bar\theta}(S_{t+1},\bar a^*_{t+1}))$
  - $D_{KL}(\Phi_z d_t'||d_t)$

#### Noisy Nets

- $\epsilon$-greedy has limitations in games, where many actions must be executed to collect the first reward. Hence, Noisy Nets proposed a noisy linear layer that combines a deterministic and noisy stream,

$$
y=(b+Wx)+(b_{noisy}\odot\epsilon^b+(W_{noisy}\odot\epsilon^w)x)
$$

### Approach

####The Integrated Agent

- Replace the 1-step distributional loss with a **multi-step variant.**

$$
d_t^{(n)} \equiv (R_{t}^{(n)}+\gamma_{t}^{(n)}\mathbf{z},\mathbf{p}_{\bar\theta}(S_{t+n},\bar a^*_{t+n}))
$$

- The resulting loss is $D_{KL}(\Phi_z d_t^{(n)}||d_t)$

- **Double Q-Learning:** using the greedy action in $S_{t+n}$ selected according to the *online network* as the bootstrap action $a^∗_{t+n}$, and evaluating such action using the *target network*. 

- **PER:** all distributional Rainbow variants prioritize transitions by the KL loss, since this is what the algorithm is minimizing: $p_t \propto (D_{KL}(\Phi_z d_t^{(n)}||d_t))^\omega$. The KL loss as priority might be more robust to noisy stochastic environments because the loss can continue to decrease even when the returns are not deterministic.

- **Dueling Network**: 

  - Shared representation $f_\xi(s)$, which is fed into **value stream** $v_\eta$ with $N_{atoms}$ outputs, and into **advantage stream** $a_\xi$ with $N_{atoms}\times N_{atoms}$ outputs, where $a_\xi^i(f_\xi(s),a)$ denotes the output corresponding to atom $i$ and action $a$.
  - For each atom $z^i$, the value and advantage stream are aggregated, as in dueling DQN, and then passed through a softmax layer to obtain the normalized parametric distributions used to estimate the returns' distributions:

  $$
  p_\theta^i(s,a)=\frac{\exp(v_\eta^i(\phi)+a^i_\psi(\phi,a)-\bar a_\psi^i(s))}{\sum_j\exp(v_\eta^i(\phi)+a^i_\psi(\phi,a)-\bar a_\psi^i(s))}
  $$

  

  - where $\phi=f_\xi(s)$ and $\bar a_\psi^i(s)=\frac{1}{N_{actions}}\sum_{a'}a_\psi^i(\phi,a')$

- We then replace all linear layers with their noisy equivalent described before. Here, we use **facrotised Gaussian noise** to reduce the number of independent noise variables.

