---
title: An overview of gradient descent optimisation algorithms
created: '2020-02-08T03:44:25.098Z'
modified: '2020-02-08T04:53:01.848Z'
---

# An overview of gradient descent optimisation algorithms

## basics
$\theta = \theta - \eta \nabla_\theta J(\theta)$
- parameters $\theta$
- objective $\nabla_\theta J(\theta)$
- learning rate $\eta$

## challenges

- choosing a learning rate
- learning rate scheduling
- per-parameter learning rate
- escaping local minima and saddle points

## algorithms

### momentum
escape local minima

$v_t = \gamma v_{t-1} + \eta \nabla_\theta J(\theta)$
$\theta = \theta - v_t$
- maintain $\gamma$ of previous gradient
- accelerate SGD in relevant directions

### Nesterov
correct gradient evaluation of momentum 

$v_t = \gamma v_{t-1} + \eta \nabla_\theta J(\theta - v_{t-1})$
$\theta = \theta - v_t$
- take step in direction of previous gradient ($\gamma v_{t-1}$)
- then correct for current situation ($\eta \nabla_\theta J(\theta - v_{t-1})$) 

### Adagrad
change $\eta$ of a parameter inversely proportional to how large its gradients have been

$g_{t,i} = \nabla_\theta J(\theta_{t,i})$
$\theta = \theta - \frac{\eta}{\sqrt{G_{t,ii} + \epsilon}} \cdot g_{t,i}$
- $i$ indexes a single parameter
- $G_{t,ii}$ is the sum of squares of gradients of that parameter up to time $t$
- weakness: gradients accumulate in $G$ so eventually all grads go to 0

### RMSprop
avoid accumulated gradients by decaying a running average, giving recency bias

$E[g^2]_t = \gamma E[g^2]_{t-1} + (1-\gamma)g^2_t$
$\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t}$
- running sum of squares of grad
- $\gamma = 0.9, \eta = 0.001$ are good values

### Adadelta
fix update units not matching by replacing learning rate with running sum of parameter updates

$E[\Delta\theta^2]_t = \gamma E[\Delta\theta^2]_{t-1} + (1-\gamma)\Delta\theta^2_t$
$\Delta\theta_t = -\frac{RMS[\Delta\theta]_{t-1}}{RMS[g]_t}$
 
- shorthand $RMS[x]_t = \sqrt{E[x^2]_t} + \epsilon$ 
- let $\theta_{t+1} = \theta_t + \Delta \theta_t$
- should be $RMS[x]_t$ but we approximate it with $RMS[x]_{t-1}$

### Adam
combine past gradients of momentum and past squared gradients of Adadelta/RMSprop

$m_t = \beta_1 m_{t-1} + (1 - \beta_1)g_t$ 
$v_t = \beta_2 v_{t-1} + (1 - \beta_2)g_t^2$ 
- estimate the mean and variance respectively

$\hat m_t = \frac{m_t}{1-\beta_1^t}$
$\hat v_t = \frac{m_t}{1-\beta_2^t}$
- correct for bias towards 0 since $m,v$ are init to 0

$\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{\hat v_t} + \epsilon} \hat m_t$
- behaves like a heavy ball *with friction*
- $\beta_1 = 0.9, \beta_2 = 0.999$ good defaults

### Adamax
replace Adam $l_2$ norm with $l_\infty$

$v_t = \beta_2^\infty v_{t-1} + (1 - \beta_2^\infty)|g_t|^\infty$
- no need for correction because does not bias towards 0
- also stable

### Nadam
add nesterov to Adam, correct for true $\hat m_t$ 

$\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{\hat v_t} + \epsilon} (\beta_1 \hat m_t + \frac{(1 - \beta_1)g_t}{1 - \beta_1^t} )$

### AMSgrad
fix Adam losing good variance information by taking the max instead of decaying

$\hat v_t = \max(\hat v_{t-1}, v_t)$
$\theta_{t+1} = \theta_t - \frac{\eta}{\sqrt{\hat v_t} + \epsilon} m_t$
- no need to de-bias $v_t, m_t$
- similar but sometimes worse than Adam



## citation
[web link](https://ruder.io/optimizing-gradient-descent/index.html)
```
@misc{ruder2016overview,
    title={An overview of gradient descent optimisation algorithms},
    author={Sebastian Ruder},
    eprint={1609.04747},
    archivePrefix={arXiv}
}
```
