---
title: EC3
created: '2020-10-21T18:21:39.834Z'
modified: '2020-10-22T01:29:47.873Z'
---

# EC3

## notes

This paper aims to learn zero-shot communication through robotic arm movement between agents aided by a zipf distribution of inputs, an energy cost for movement, and a feature extraction step. The idea of using a 3D robots to communicate is pretty novel for the deep learning era and would be interesting to see how you can bring in some classical emergent communication ideas (Steels) into the deep learning era. As well, there is a very interesting idea of using a third party to achieve zero-shot communication and it feels overlooked as a contribution whereas it could be quite powerful. In total, there seem to be five separate contributions (1) deep RL for 3D robotic communication (2) zipf distribution for zero-shot (3) energy cost for zero-shot (4) feature extraction for zero-shot (5) third party observers. Sadly, the work doesn't feel cohesive and it isn't clear what the direction is or why these contributions are important.

The paper's motivation is interesting but I don't fully understand what the goal is and whether the parameters of the experiments are aligned with it. If the experiment is about showing zero-shot communication using a zipf distribution for intents and an auxiliary energy cost, you can just as easily apply both of things to a discrete (or continuous) sender-receiver game. If the goal of the paper is to demonstrate the efficacy of using a 3D environment or FK to aid in zero-shot communication, then there should be comparisons to linguistic communication protocols. If the goal is simply zero-shot communication, then there should be comparisons to other zero-shot baselines (Other Play). It would help to understand the exact issue being studied and why the parameters of the experiment are appropriate.

The background/related work section is too brief and though the introduction provides some detail it would be useful to compare and contrast this work directly against previous work. For example, it is mentioned that Other Play (Hu et al, 2020) was used only on discrete spaces but as far as I understand their method can easily extend to continuous spaces. It would be worth delving into why specifying symmetries of trajectories is non-trivial and warrants new methods. It would also be useful to contrast this method with previous auxiliary "energy" losses (e.g. Mordatch and Abbeel, 2018).

The problem setting is written well and clearly but it might also help to add the formal definition of a trajectory for an unfamiliar reader. Other details to add would be the type of models and their details. I could only find they were 3-layer FC networks in the appendix but couldn't find the width of the layers, nor other important hyperparameters to understand the work: optimizer, learning rate, what constitutes an epoch (is it an iteration?). Furthermore, I would be interested in understanding the reasoning behind going straight to an experiment through a differentiable FK. Since agents receive motion trajectories which are continuous vectors anyways, why not skip FK and just send sender's actions to the receiver as a message? This would yield action sequences not trajectories but may be just as good and easier to optimize. Using 3D motion adds a large level of complexity to the analysis and it is not clear why.

The experiments are generally interesting but I'm not sure that all the results justify their conclusions.

Section 5.1 

The experiment claims to show that zipf distribution helps zero-shot communication but I am skeptical because this could be a case of confused baselines. With the uniform distribution, a receiver should get 10% accuracy guessing a random intent. With the zipf distributions, guessing the most common intent should give much higher 34% accuracy. Furthermore, if intents are not randomized, we expect a bad zipf receiver to be always guessing the first, most common intent. Therefore, we expect the zipf case to have higher average accuracy. This feels like a simpler explanation of why zipf agents have higher non-training partner accuracy than arguiung that it is evidence of zero-shot generalization. It seems to me that this experiment shows that agents have learned the underlying distribution and that zipf is more informative than the uniform. Another experiment that would counteract this hypothesis would be very useful.

It may be worth using "receiver" only for training partners and "observer" for the third party observer. I deduced "observers" meant receivers in the context of Fig 2a but this wasn't clear. 

It is mentioned that you need "additional third-party observer training, to increase the likelihood of communication generalization" but third party observers were previously introduced as an evaluation method not an additional training method so this is confusing. I explain my thoughts on the observer method below.

Section 5.2 

The authors introduce the idea of a third-party observer. This agent learns from the "train" community of frozen sender agents and is then evaluated on the "test" community. In the work, it seems like this is used as a proxy for evaluating zero-shot communication ("Third-Party Observer Evaluation"). As an evaluation metric for how the agents are doing, this choice is not justified nor is it valid.  Your observer is measuring the difference in distributions between your train population protocols and your test population protocols. It could easily learn the distribution of communication protocols even if your communication protocols are all very different. In essence, you can have a perfect third party observer accuracy while having agents that have no zero-shot communication success. Your observer is, at best, qualitatively measuring the range of communication protocols you learn. This metric is not quantitative and highly-dependent on the observer. If you want to measure zero-shot communication, then test it directly.

BUT, if this is seen instead as a secondary training step to learn a more general receiver, it is very interesting (and has connections to work in iterated learning!) I imagine that the authors mean the latter but the paper feels like it implies the former. This feels like it skips over what could be a very interesting contribution. I would really like to see this method evaluated on something like the lever game of Hu et al (2020).

Another difficulty in understanding the paper was that Phi, the feature extractor, is not defined (or not clearly defined). Is it a hand-crafted feature or is the learned receiver's logits? If it is the logits which receiver is used? Is it an average over all receivers or is it the receiver of the corresponding sender that is being tested? None of this is made clear which makes it difficult to judge the results. 

Furthermore, using just two possible inputs (intents) feels insufficient for demonstrating any general points. Especially when choosing just one of them already gets 67% accuracy. I think the experiments on N=2 can be scrapped in favour of N=10 and a comparison to Other Play or some other zero-shot baseline to demonstrate the efficacy of this approach.

Section 5.3

The results in Table 2 are not very strong. We see that for N = 10 without curriculum all performance is below the non-communication baseline (guessing the most probably class). This is confusing and feels like it directly contradicts previous efforts that used the observer and feature extractor. I don't like the explanation that this is just "getting stuck in a local optimum" since there are infinitely many local optima for a communication protocol in a continuous space. As demonstrated by Hu et al (2020), the goal of a zero-shot communication technique is to reduce those optima so you don't get stuck. It is a little concerning that this method without curriculum doesn't scale to even 10 inputs.

As well, it is difficult to understand why the training scheme can lead to performance worse than the non-communication baseline. It would be helpful to establish different baselines to better understand the results in context.

I had a hard time following the discussion on energy values and intents. If the goal of the method is to ensure the energy levels of the intents follow the same zipf distribution ordering, would it be sufficient to just increase the coefficient of the energy loss or modify the objective function in another way to guarantee this? 

The idea of a curriculum is interesting but I also didn't fully understand how senders were pretrained. Were they only trained with the energy loss? For how many epochs or until what point? I wasn't sure whether there is an "energy-based implicit latent structure" induced or whether this two-stage optimization is simply getting around some sort of optimization issue when optimizing together. 

Finally, I am worried that this method may fail on N=20 since each modest increase in scale has lead to a new trick being necessary. It would help to see further experiments in the appendix on this but I understand if the authors wish to leave that to future work.

Final Notes

There are many questions for this work but there is a potentially interesting line of research here. I recommend the authors elucidate a bit more about their models, setup, and hyperparameters. It would also be great to double-check the evaluation schemes with further experiments that validate their hypotheses. Additionally, it would help readers to see better baselines that contextualize their results and test other zero-shot techniques on their setup to make a stronger case for the proposed methods. The experiments, as they stand, do not make a strong case for 3D robotic arm communication but I can see this changing. One way is to test the same methods in sender-receiver games and then transfer lessons to the robotic domain. Another would be to get strong, relatively high input results. Overall, the paper is interesting but I think it could use some work. I feel there are many good ideas that need to be refined (again, would love to see the observer method compared to other play)

