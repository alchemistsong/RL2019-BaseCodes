# Exercise 4 - Joint Action Learner.

In this task, you are required to implement the Joint Action Learning algorithm. In general, this algorithm follows similar steps with the Q-Learning algorithm. However, unlike Q-Learning, the Q-tables are defined for joint actions. Additionally, this algorithm also maintains a model of opponent behaviour which will be used to calculate the updates during the training process. 

The opponent model should be based on the actions of opponent agents observed at a particular state. In this case, for each state, the model should store an empirical distribution of observed opponent behaviour under each state. When the state has yet been visited, initialize the opponent model using a discrete uniform distribution.

For the state-joint action value table, implement updates that are still the same with your usual Q-Learning update. However, unlike Q-Learning, the maximum next state value is computed as the value of the best action that the modelling agent can take in the next state. The value of the best action is the the expected value of action under the distribution of actions that the opponent can take. In turn, the distribution of actions that the opponent can take is calculated based on the opponent model the agent have learnt.

For a concise description of the algorithm, refer to Table 4 from the works of [**Bowling and Veloso, 2001**](http://www.cs.cmu.edu/~mmv/papers/02aij-mike.pdf).

## Specifications
### Automarking requirements
To ensure that your codes can be used by the marking script, ensure that all the necessary functions have been implemented. To check whether these implementations are correct, you **must** use the code snippet given in `__main__` to test your implementation. This code snippet provides an outline of how your implemented functions will interact to train a team of agents using Joint Action Learning. If you just modify the number of episodes and the hyperparameters for training, you can use this code to train a team of Joint Action Learning agents.

Additionally, **ensure** that the initial Q-Values of all state-action pairs are initialized to **zero** prior to training. Although you can technically use any initialization value for Q-Learning, we require this as a means for unit testing your implementations.

### Implemented Functions
#### `__init__(self, learningRate, discountFactor, epsilon, numTeammates)`
This init function should initialize all the necessary parameters for training a Q-Learning Agent. This includes the learning rate, discount factor, the epsilon value (if you use an epsilon greedy exploration strategy), and the number of teammates that the agent is interacting with. This function will only be called once at the very beginning when you initialize agents for training.

#### `learn()`
This is the most important function you need to implement in this task. This function has no input parameters. On the other hand, this method **must** return a single scalar that specifies the change **(value after update subtracted by value before training)** in updated state-action value after you've trained your agents using Joint Action Learning's update. It will be used by the automarker to compare the correctness of your implementation against the solution.

#### `act()`
This function will be used to choose the actions that your agents will use when faced with a state. It should only return the action that should be taken by the agent at the current state.

#### `toStateRepresentation(state)`
You might want to use a different representation compared to the ones provided by the environment. This will provide a problem to the automarker. Therefore, you should implement a function that maps the raw state representation into the the state representation that you are using in your implementation. This function will receive a state and outputs it's value under the representations that you are using in your implementations. Additionally, this state representation **must be able to be used as keys of a python dictionary** since the marking tools will use this to check the correctness of your algorithm.

#### `setState(state)`
This function will be used to provide the agents you're controlling with the current state information. It will receive the state representation from the environment as an input. On the other hand, this does not need to output anything.

#### `setExperience(state, action, opponentActions, reward, status, nextState)`
Once an agent executes an action, it will receive the rewards, status, and next states resulting from that action. Additionally, unlike single-agent Q-Learning, once all agents have decided on an action to take, an agent will also receive information on actions that are taken by opponents. Use this method to set these data to prepare your agent to learn using the Joint Action Learning update.

#### `reset()`
You might want to reset some states of an agent at the beginning of each episode. Use this function to do that. This function does not require any inputs. Additionally, it also does not provide any outputs.

#### `setLearningRate(learningRate)` and `setEpsilon(epsilon)`
This function should be used to set the learning rate and the epsilon (if you are using epsilon greedy) that you use during training. 

#### `computeHyperparameters(numTakenActions, episodeNumber)`
This function should return a tuple indicating the learning rate and epsilon used at a certain timestep. This allows you to schedule the values of your hyperparameters and change it midway of training.

### Training process
To see how your implemented function interact with each other to train the agent, check the `__main__` function inside `JointLearningAgent.py`. Make sure that you can successfully train your agent using the **exact same** codes inside `__main__` to ensure that your implementations are correct. This snippet of code used in `__main__` is also going to be used in the marking process.

## Marking details
### Performance marking
Using similar codes as what you've seen in `__main__`, we are going to run your agent on a randomly sampled environment and compare it's performance to our solution. Performance is measured by running an experiment using your implementation. We then divide the sequence of episodes into groups of consecutive episodes and average the reward of the agent on these groups.

### Unit test marking
We compare the results of updates from `learn()` for unit testing.
