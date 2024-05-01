# Scaffold: Breaking Down Complex Tasks to Aid Reinforcement Learning

Team Members: Luigi Victorelli, Elijah Walker, Sophie Tran, Mathew Biji

Research Lead: Naveen Mukkatt


# Poster

![Poster](https://github.com/ACM-Research/Scaffold/assets/33157393/a25276ab-c9b6-42e3-85d2-49a58b593a8a)

# Introduction
When solving complex problems in the world of reinforcement learning, AI models, incentivized by reaching the highest reward possible, often exhibit unwanted behaviors. Our research project, Scaffold, demonstrates a possible solution: simplify the complex tasks, and ask the agent to synthesize the tasks together over time, allowing fine control over more complex behaviors. This is similar to scaffolded learning, where educators give less and less support as a person's competence grows. As we train AI-controlled airplanes in Unreal Engine using reinforcement learning, we simplify complex tasks into manageable levels, slowly increasing the difficulty of the task over time in stages, eventually taking the AI from being unable to fly to pulling off dogfights in the air.


# Creating the Game
We designed the game environment in Unreal Engine 5. The team created a simulation of a plane by designing the plane's 3D model and its hitboxes, implementing its controls, and creating obstacles in the form of terrain.

## Unreal Engine 5
Unreal Engine is a game engine developed by Epic Games. First designed in 1998, it has gone through multiple generations, with the most recent one being Unreal Engine 5 that released in 2022. The engine is written in C++, and in Unreal Engine 4, a visual scripting system called Blueprint was included, which made coding game logic much simpler for non-programmers.

# Training an Agent

## Reinforcement Learning
We used reinforcement learning to train our AI. Reinforcement learning is the process by which an AI known as an *agent* learns to complete a task in an environment. It does this by observing its environment and choosing an action that maximizes its reward. Rewards are given from the environment, and it is a measure of the agent's success. 

### Observations
The *game state* is the current state of the environment; it contains all information about the game that describes the environment, and the list of possible states is known as the *state space*, often represented *S*. A single game state can be referred to with  lowercase *s*.

It is often completely impractical to give the agent the entire game state; there are too many parameters and it would be very computationally expensive, especially in our situation where we simulate a 3D world. Instead, we derive *observations*, which simplify the state down. In our case, the agent can make the following observations:

- Plane Direction/Velocity
	- These measurements were egocentric, meaning that the parameters describing these values were with respect to the reference frame of the plane pointing in its direction of movement.
- Distance to Ground (single value)
- Distance to Ceiling (single value)
- Enemy Plane Direction/Velocity
	- This allowed the plane to know at all times where the enemy plane was. In later experiments and with better computational hardware, this may be removed to necessitate the plane finding its opponents.
- Distance to Obstacle in Front
	- This observation was done by a cone of raycasts extending from the front of the plane and pointing forward. When the raycast collided with an object, it would return the distance of the object.

### Actions
When the agent observes its environment, it is able to take *actions* that allow the agent to manipulate it in some way. The agent selects actions according to a *policy*, that determines the strategy for selecting an action based on the current state. Policies are represented as $\pi(a|s)$, where $a$ is the action taken conditional to the state $s$. 

The list of actions the plane can take are the following: 
- Turn left/right
- Pitch up/down
- Roll left/right
- Throttle/Brake
### Rewards
Rewards are the reinforcement in reinforcement learning. They give the agent an indicator of how well it is performing in an environment. Rewards are a scalar, real value, and the goal of the agent is to learn a policy that maximizes the total reward an agent gets in its environment. 

The reward function can be denoted as $R(s, a, s')$. $s$ is the initial state, $a$ is the action the agent takes, and $s'$ is the state the environment moves to after the action is taken. The agent will receive a reward according to this function.

The list of rewards we use are the following:

- Down a plane: +10
- Crash: -10
	- Negative rewards are also called *penalties*, and this penalty heavily disincentivizes crashing.
- Fire bullets: +0.1
	- This is to incentivize the plane to fire more, increasing the chance that it experiments and finds a way to down another plane.
	- A major difficulty with this reward was that only continuous actions were compatible with Learning Agents. Our designated method to solve this was to assign a continuous variable to the action of firing bullets. Whenever this variable went above a threshold, the plane would fire. This simulated a discrete variable (actually firing the bullet). Therefore, the policy would adjust the continuous variable's value, indirectly adjusting its policy for firing bullets.
- Velocity bonus: +0.2 $\rightarrow$ 0
	- This reward encourages the plane to fly faster, which is useful in the initial stages when it is trying to stay in the air. However, higher velocities made it difficult for the plane to aim, so after the first stage of training, we removed this reward.

### Algorithm
The algorithm we use is Proximal Policy Optimization (PPO). This algorithm was chosen because of its performance and ease of use; Unreal Engine has a plugin designed to support PPO. It is a policy gradient method that trains the policy network of the agent, iteratively improving the policy over time to reach a near-optimal solution. 

PPO uses a stochastic policy, meaning that instead of a policy having a set action for each state, there is a probability distribution of actions for each state. As training progresses, the probability distribution of actions tends towards better outcomes for the agent.

## Unreal Engine's Learning Agents

In early 2023, Epic Games revealed Learning Agents, a machine learning plugin allowing creators to train AIs with reinforcement or imitation learning. Learning Agents came with an implementation of PPO and the ability to add agents, commonly-used observations like angular velocity or collision monitors, conditional rewards, and common actions like rotation. Learning Agents required Unreal Engine 5.2+, and only worked on Windows computers, which presented a hurdle to our team; however, it greatly simplified our task to focus on the process of scaffolded learning.

## Scaffolded Training
Scaffolded learning in humans implies decreasing levels of support from the trainer over time. To do this with agents, we first teach the plane the absolute basics: learn to fly without crashing into terrain. For this, we gave the plane a velocity reward, which incentivizes the plane to move faster and avoid stalling in midair, which would cause a crash. 

In the next stage, we taught it to fire at stationary targets. Moving targets were too difficult, and stationary targets were fairly small in comparison to the world. We had to work around Learning Agents only supporting continuous actions, and firing bullets is a discrete action, but we worked around that (described in Actions).

Finally, the last stage we built was for the airplane to fight another airplane. The added difficulty in the stage was to track a moving target while dodging fire. We chose not to give the airplanes knowledge of bullets being fired at it, instead encouraging the plane to predict where the other airplanes would fire. Bullets only fired straight ahead, simplifying this problem.

# Conclusion
The goal of the project was to mitigate unwanted behaviors and slowly build up a skillset for the task of aerial combat with another agent. The results in the end were largely positive, but with a few notable downsides. The plane exhibited erratic movement even when not trying to dodge bullets. This is most likely due to the fact that there is no penalty for excessive movement like there is in real life (fuel costs). This could be solved with adding a penalty for turning or accelerating. In addition, setting up multiple stages for training took significantly more time than creating one stage.

The scaffolded approach to reinforcement learning allows for much finer control over the behavior of agents. This approach can be more time-consuming to set up properly; however, it may result in a much better policy for the model and easier training. 


# References
 
Wood, D. J., Bruner, J. S., \& Ross, G. (1976). The role of tutoring in problem solving. Journal of Child Psychiatry and Psychology, 17(2), 89–100.
  
Mnih, V., Kavukcuoglu, K., Silver, D., et al. (2015). Human-level control through deep reinforcement learning. Nature, 518(7540), 529–533. https://doi.org/10.1038/nature14236

Holden, D., \& Mulcahy, B. (2023, September 20). Learning to Drive. Epic Games. https://dev.epicgames.com/community/learning/tutorials/qj2O/unreal-engine-learning-to-drive 


Schulman, J., Wolski, F., Dhariwal, P., Radford, A., \& Klimov, O. (2017, August 28). Proximal policy optimization algorithms. arXiv.org. https://arxiv.org/abs/1707.06347 

# Demo
[![Video](https://img.youtube.com/vi/2w9B1pQT_qk/0.jpg)](https://www.youtube.com/watch?v=2w9B1pQT_qk)




