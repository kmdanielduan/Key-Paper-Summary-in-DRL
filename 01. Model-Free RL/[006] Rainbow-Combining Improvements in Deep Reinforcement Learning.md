# Rainbow-Combining Improvements in Deep Reinforcement Learning

**Authors**: Matteo Hessel, Joseph Modayil, Hado van Hasselt, Tom Schaul, Georg Ostrovski, Will Dabney, Dan Horgan, Bilal Piot, Mohammad Azar, David Silver

**Year**: 2017

**Links:** [[arxiv](https://arxiv.org/abs/1710.02298)] [[summary]]

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

- Refer to [001] Playing Atari with Deep Reinforcement Learning, Mnih et al, 2013. **Algorithm: DQN.** [[arxiv](https://arxiv.org/abs/1312.5602v1)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01. Model-Free RL/[001] Playing Atari with Deep Reinforcement Learning.md)]

#### Double Q-Learning

- Refer to [004] Deep Reinforcement Learning with Double Q-learning, Hasselt et al 2015. **Algorithm: Double DQN.** [[arxiv](https://arxiv.org/abs/1509.06461)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01. Model-Free RL/[004] Deep Reinforcement Learning with Double Q-learning.md)]

#### Prioritized Experience Replay

- Refer to [005] Prioritized Experience Replay, Schaul et al, 2015. **Algorithm: Prioritized Experience Replay (PER).** [[arxiv](https://arxiv.org/abs/1511.05952)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01. Model-Free RL/[005] Prioritized Experience Replay.md)]

#### Dueling DQN

- Refer to [003] Dueling Network Architectures for Deep Reinforcement Learning, Wang et al, 2015. **Algorithm: Dueling DQN.**[[arxiv](https://arxiv.org/abs/1511.06581)] [[summary](https://github.com/kmdanielduan/Key-Paper-Summary-in-DRL/blob/master/01. Model-Free RL/[003] Dueling Network Architectures for Deep Reinforcement Learning.md)]

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



### Approach

#### 

### Discussion and Others
