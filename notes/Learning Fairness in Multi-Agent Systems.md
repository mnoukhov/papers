# Learning Fairness in Multi-Agent Systems

## overview

MARL algorithm to create fairness in a somewhat distributed way

change reward to take into account agent reward with respect to average reward

gossip algorithm to make sure 

## methods

### fair efficient reward

$$r_t^i = \frac{\bar u_t / c}{\epsilon + | u_t^i/\bar u_t - 1| }$$

- where $c$ is a constant to normalize the numerator (?)  set to the max reward per timestep
- numerator is resource utilization
- denominator is this agent's deviation from average reward

### heirachical policy

## experiments

### 

## issues

if we're sharing weights, that's already not really distributed isn't it

