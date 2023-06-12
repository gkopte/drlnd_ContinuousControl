# Design Choices

The agent utilizes Deep Deterministic Policy Gradients (DDPG) - an algorithm which concurrently learns a Q-function and a policy. It uses off-policy data and the Bellman equation to learn the Q-function, and uses the Q-function to learn the policy. This dual mechanism allows for efficient learning in environments with high-dimensional state and action spaces. The agent applies a replay buffer to stabilize learning by storing and reusing past experiences, reducing correlations between consecutive steps. 

Noise is added to the actions for exploration purpose, following an Ornstein-Uhlenbeck process, which generates temporally correlated exploration suitable for problems with inertia.

## Hyperparameters' list

- Replay Buffer Size: 1e6
- Minibatch Size: 128
- Discount Factor (GAMMA): 0.95
- Tau for Soft Update of Target Parameters: 1e-3
- Learning Rate of the Actor: 1e-4
- Learning Rate of the Critic: 1e-3
- L2 Weight Decay: 0
- Number of Agents: 20
- Training Frequency: Every 20 steps

# Agent

The agent is implemented with the following key methods:

**<u>act</u>**: Takes in a state and returns actions for each agent according to the current policy. It also adds noise for exploration.

**<u>learn</u>**: Takes in a batch of experiences and performs learning on both actor and critic networks. It also updates the target networks.

**<u>soft_update</u>**: Applies soft update to the target networks' parameters using the local networks' parameters.

**<u>step</u>**: Takes in state, action, reward, next state, and done information, stores them in the replay buffer, and performs learning every few steps.

# Network

The Actor and Critic networks are each composed of two hidden layers with 128 and 256 nodes respectively. Both networks use ReLU activation functions for the hidden layers. Moreover, Batch Normalization is applied to the inputs of each layer of both networks to stabilize learning and reduce generalization error.

The output layer of the Actor uses a tanh activation function to output actions. The Critic's output layer is a single node to output the state-action value function. 


# Replay Buffer

The replay buffer is implemented as a fixed-size deque where new experiences replace the oldest ones when the buffer size limit is reached. The experiences are stored as tuples of states, actions, rewards, next states, and done flags.

# Ideas for Future Work

- Fine-tuning hyperparameters or the network architecture, including the size and number of layers in the neural network, could potentially improve performance.
- Implementing prioritized experience replay could potentially speed up learning by sampling important experiences more frequently.
- Another potential improvement could be the use of a different noise process for exploration, or tuning the parameters of the Ornstein-Uhlenbeck process.
