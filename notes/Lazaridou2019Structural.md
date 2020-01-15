## Citation
Structural and functional learning for learning language use
Angeliki Lazaridou∗, Anna Potapenko∗, Olivier Tieleman
VIGiL Workshop @ NeurIPS 2019

## Overview

How to learn natural language as a function
- supervised language understanding isn't functional
- emergent communication isn't natural
- pre-trained LM + fine-tuned multi-agent causes language drift

Use a pre-trained LM to generate sentences and make your RL agent re-rank them

Caveat: syntactic language drift doesn't exist, but semantic is still there!
