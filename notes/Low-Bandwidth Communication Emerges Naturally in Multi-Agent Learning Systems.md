---
attachments: [Clipboard_2020-10-22-00-24-51.png]
tags: [emergent, marl]
title: Low-Bandwidth Communication Emerges Naturally in Multi-Agent Learning Systems
created: '2020-10-22T04:20:45.255Z'
modified: '2020-10-22T17:51:02.338Z'
---

# Low-Bandwidth Communication Emerges Naturally in Multi-Agent Learning Systems

## overview

- proposes a spectrum from low-bandwidth (pheromones) to high-bandwidth (language) communication
- shows that existing MARL algorithms effectively learn low-bandwidth communication

## communication spectrum

![](@attachment/Clipboard_2020-10-22-00-24-51.png)

in order to forage or get food, you need to collaborate
- ants use pheromone trails
- hunting animals use gestures, chemicals, posture
- sound animals tell you where food is
- smart animals communication for group cohesion

## setup

MARL pursuit-evasion game on torus, $N$ pred, 1 prey
- prey faster than predators
- actions are velocity + angle

Prey strategy is maximize distance and angle between predators (equidistant from all)

Predator strategy is 
- learned neural network with DDPG observes all agents
- baseline max-min of the prey

## results

when predator is faster than prey, both strageies work

as predator velocity decreases, DDPG outperforms 


## citation
