# Simulating Saramago's Blidness (Watts-Strogatz model)

This project is based on José Saramago’s book “Blindness.” It simulates the spread of sudden, unexplained blindness in a city and explores how the resulting societal collapse affects individual and group survival. The model investigates how different social structures, individual capabilities, and group compositions impact resilience in the face of crisis. The goal is to evaluate how the spread of blindness disrupts social networks, increases risk, and alters survival chances, especially when key individuals (like sighted people) are present in a group.

## Model Description
Individuals are represented as nodes in a small-world network generated using the Watts-Strogatz model (n=population, k=4, p=0.1), capturing the clustering and short path lengths typical of real social systems. Each individual can be susceptible, infected, resistant, blind (newly/adapted), or dead, and their relationships (edges) evolve over time based on survival dynamics and social adaptation. The model follows a synchronous update scheme: at each time step, all nodes simultaneously update their states based on the current configuration. This setup allows for clear cause-and-effect propagation across the network. Randomness is introduced via a Monte Carlo approach, where key processes (infection, adaptation, death) depend on stochastic draws, making repeated simulation runs necessary for observing emergent patterns and variability.

### Parameters
- Population: Number of individuals (nodes) on the network. Set to 100.
- Time Steps: Representing days in the simulation. Set to 100. I ran the simulation with other values and noticed a stabilization before the 100th time step in all cases.
- Initial Infection Fraction: Number of individuals from the population infected with blindness in step 0. Set to 10.
- Initial Resistant Fraction: Number of individuals from the population resistant to blindness in step 0. Set to 5 as default, but swiped on the empirical analysis.
- Blindness Adaptation Time: Time needed for infected individuals to get used to being blind (turning into adapted blind). Set to 10 time steps (10 days).
- Blindness Status Change: Infected nodes only change to Newly Blind after 3 time steps.
- Probability of Adaptation to Blindness: Probability used to determine how likely an infected individual is to adapt to blindness. Set to 0.3 as default, but swiped on empirical analysis.
- Probability of Getting Infected: Probability used to determine how likely an infected node is of infecting a susceptible node. Given this is an apocalypse scenario, the probability was set to a high value, 0.7.
- Death Status Change: A node normally dies when it is somewhat isolated/is not on an advantageous cluster, and when we can assume that after a certain number of days, it would die in real life:
- Low Survival Rate + No Strong Bonds (after 10 time steps): If a node maintains low survival rates (<=0.2) and has no strong bonds (weight >= 0.7) for 10 time steps, it dies;
- No connections + blindness: if a node is blind (new or adapted) and has no connections, it dies;
- Majority of neighbors are newly blind (for 3 or more time steps): If a node has the majority of its neighbors as newly blind for 3 or more steps (days), it dies.
- Beta represents how fast a node changes its bond strength with others. If a bond is smaller than 0.05, the bond breaks. Beta is a number between 0 and 1. Given the apocalypse scenario, beta was set to 0.4. This number aims to represent a balance between wanting to maintain a certain number of individuals around to survive, but also being fast enough in the need to break ties with uncooperative nodes.
- Gamma represents the “intolerance” of a node, how much the node cares about the difference of opinions. If gamma is smaller than or equal to 1, then edge weights converge to 1. If gamma is bigger than 1, then edge weights decrease if the nodes' “opinions” are different enough. Gamma is always bigger than 0. Gamma was set to 0.5, a moderate intolerance, which avoids automatic convergence or immediate breakdown.
- Alpha is a parameter that affects how influential people are (how fast they change their opinion). Here, it determines how fast nodes adapt to others' survival rates (if they become more or less cooperative). Alpha has to be between (0, 0.5], and was set as 0.05 by default, aiming to preserve memory of past resilience levels.

### Variables
- Status: Nodes can be susceptible, infected, or resistant.
- Blindness Stage: Infected nodes can be newly blind, adapted blind, or sighted.

### Update Rules
- Edge Weights (wij): Starts with a small-world scenario where edges with >0.7 weight represent strong bonds (like family). Impacted by survival rate, and resource-finding probability wij = wij(1-wij)(1-|ci-cj). wij is between (0, 1).
- Survival Rate: ci=wij(cj-ci), each individual (nodes) has a survival rate ∈ (0, 1.0) that determines their chances of survival during the pandemic. Survival rate here represents the change of opinion of a node when interacting with another node.

# Full Report
https://docs.google.com/document/d/1n-hQt_ypai0i0R_QGfQR_B-h4B1aFAFOOg_1Ta7h5lY/edit?usp=sharing
