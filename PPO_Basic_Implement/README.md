# PPO for Beginners

## Introduction

Please note that this PPO implementation assumes a continuous observation and action space, but you can change either to discrete relatively easily. I follow the pseudocode provided in OpenAI's Spinning Up for PPO: [https://spinningup.openai.com/en/latest/algorithms/ppo.html](https://spinningup.openai.com/en/latest/algorithms/ppo.html); pseudocode line numbers are specified as "ALG STEP #" in [ppo.py](./ppo.py).

## Usage
First I recommend creating a python virtual environment:
```
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

To train from scratch:
```
python main.py
```

To test model:
```
python main.py --mode test --actor_model ppo_actor.pth
```

To train with existing actor/critic models:
```
python main.py --actor_model ppo_actor.pth --critic_model ppo_critic.pth
```

NOTE: to change hyperparameters, environments, etc. do it in [main.py](main.py); I didn't have them as command line arguments because I don't like how long it makes the command.

## How it works

[main.py](main.py) is our executable. It will parse arguments using [arguments.py](arguments.py), then initialize our environment and PPO model. Depending on the mode you specify (train by default), it will train or test our model. To train our model, all we have to do is call ```learn``` function! This was designed with how you train PPO2 with [stable_baselines](https://stable-baselines.readthedocs.io/en/master/) in mind. 

[arguments.py](arguments.py) is what main will call to parse arguments from command line.

[ppo.py](ppo.py) contains our PPO model. All the learning magic happens in this file. Please read my [Medium series](https://medium.com/@eyyu/coding-ppo-from-scratch-with-pytorch-part-1-4-613dfc1b14c8) to see how it works. Another method I recommend is using something called ```pdb```, or python debugger, and stepping through my code starting from when I call ```learn``` in [main.py](main.py). 

[network.py](network.py) contains a sample Feed Forward Neural Network we can use to define our actor and critic networks in PPO. 

[eval_policy.py](eval_policy.py) contains the code to evaluating the policy. It's a completely separate module from the other code.

[graph_code](graph_code) directory contains the code to automatically collect data and generate graphs. Takes ~10 hours on a decent computer to generate all the data in my [Medium article](https://medium.com/@eyyu/coding-ppo-from-scratch-with-pytorch-part-1-4-613dfc1b14c8). All the data from the medium article should still be in ```graph_code/graph_data``` too in case you're interested; if you want, you can regenerate the graphs I use with the data. For more details, read the README in graph_code.

Here's a great pdb tutorial to get started: [https://www.youtube.com/watch?v=VQjCx3P89yk&ab_channel=TutorialEdge](https://www.youtube.com/watch?v=VQjCx3P89yk&ab_channel=TutorialEdge) <br />
Or if you're an expert with debuggers, here's the documentation: [https://docs.python.org/3/library/pdb.html](https://docs.python.org/3/library/pdb.html)

## Environments
Here's a [list of environments](https://github.com/openai/gym/wiki/Table-of-environments) you can try out. Note that in this PPO implementation, you can only use the ones with ```Box``` for both observation and action spaces.

Hyperparameters can be found [here](https://github.com/araffin/rl-baselines-zoo/blob/master/hyperparams/ppo2.yml).
