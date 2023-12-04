# Scaffold: Breaking Down Complex Tasks to Aid Reinforcement Learning

Team Members: Luigi Victorelli, Elijah Walker, Sophie Tran, Mathew Biji
Research Lead: Naveen Mukkatt


# Poster

<attach poster here>

# Introduction
When solving complex problems in the world of reinforcement learning, AI models, incentivized by reaching the highest reward possible, often exhibit unwanted behaviors. Our research project, Scaffold, demonstrates a possible solution: simplify the complex tasks, and ask the agent to synthesize the tasks together over time, allowing fine control over more complex behaviors. This is similar to scaffolded learning, where educators give less and less support as a person's competence grows[INSERT REFERENCE 1 HERE]. As we train AI-controlled airplanes in Unreal Engine using reinforcement learning, we simplify complex tasks into manageable levels, slowly increasing the difficulty of the task over time in stages, eventually taking the AI from being unable to fly to pulling off dogfights in the air.


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

[OBSERVATIONS HERE]

### Actions
When the agent observes its environment, it is able to take *actions* that allow the agent to manipulate it in some way. The agent selects actions according to a *policy*, that determines the strategy for selecting an action based on the current state. Policies are represented as $\pi(a|s)$, where $a$ is the action taken conditional to the state $s$. 

The list of actions the plane can take are the following: 

INSERT ACTIONS HERE

### Rewards
Rewards are the reinforcement in reinforcement learning. They give the agent an indicator of how well it is performing in an environment. Rewards are a scalar, real value, and the goal of the agent is to learn a policy that maximizes the total reward an agent gets in its environment. 

The reward function can be denoted as $R(s, a, s')$. $s$ is the initial state, $a$ is the action the agent takes, and $s'$ is the state the environment moves to after the action is taken. The agent will receive a reward according to this function.

The list of rewards we use are the following:

LIST REWARDS HERE

### Algorithm
The algorithm we use is Proximal Policy Optimization (PPO). This algorithm was chosen because of its performance and ease of use; Unreal Engine has a plugin designed to support PPO. It is a policy gradient method that trains the policy network of the agent, iteratively improving the policy over time to reach a near-optimal solution. 

PPO uses a stochastic policy, meaning that instead of a policy having a set action for each state, there is a probability distribution of actions for each state. As training progresses, the probability distribution of actions tends towards better outcomes for the agent.

## Unreal Engine's Learning Agents

In early 2023, Epic Games revealed Learning Agents, a machine learning plugin allowing creators to train AIs with reinforcement or imitation learning. Learning Agents came with an implementation of PPO and the ability to add agents, commonly-used observations like angular velocity or collision monitors, conditional rewards, and common actions like rotation. Learning Agents required Unreal Engine 5.2+, and only worked on Windows computers, which presented a hurdle to our team; however, it greatly simplified our task to focus on the process of scaffolded learning.

## Scaffolded Approach


## #Training



# Analysis 



# Conclusion


# References




## KaTeX

You can render LaTeX mathematical expressions using [KaTeX](https://khan.github.io/KaTeX/):

The *Gamma function* satisfying $\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$ is via the Euler integral

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$



