# Corn Reinforcement Learning
Alex Yang
## Overview
- Teaching an AI to trade corn from ground zero, based on corn prices and weather.
- Using historical data, I trained a custom **reinforcement learning (RL)** agent to trade corn for profit.
- Language: **Python**
- Libraries: **OpenAI Gymnasium**, **stable_baselines3**, **pandas**, **numpy**, **matplotlib**
- Google Collabs Link: https://colab.research.google.com/drive/1hzc0-o3NRSmvXSABePQ1KiwJzx-0C2V3?usp=sharing
## Motivation
I saw a prety cool video of an AI learning to walk from ground zero, and wanted to try to replicate trading in a similar way. I wondered if an AI could pick up trends between weather and corn prices, and use it to make smart trades.
## Process
### Step 1: Gathering and formating data
For corn prices, I used **Alpha Vantage**, a free API that provided historical financial market data. Luckily, they had the option to request the data in a pandas database format. As for weather data, I used **Visual Crossing Weather**. The data came in a CSV format, so I converted it to a pandas database. I merged the two based on matching dates, and kept only the price and temperature columns for each day. Finally, I **normalized** the data using a standard **Z-score** scale, an essential step in the RL process to ensure that stability and convergence(large outlier may lead to unstable updates during training), as well as compatibility with the RL algorithm.
### Step 2: Creating a custom RL enviroment
I ended up building my custom RL enviroment based on the the **OpenAI Gymnasium** interface, which is a standard library provided by OpenAI that allows one to interact with various (RL) environments in a consistent and unified way. In my case, it was really helpful since it took care of all the behind the scenes processing, and let me focus on the code that would shape the RL agent's enviroment--- namely, their actions, their observation space, and their reward scoring.
Actions: The actions the RL agent could take each step. 1) Buy. 2) Sell. 3) Hold.
Observation Space: The information the RL agent could access each round, and base their decisions and pattern recognization off of. In this case, the RL agent was given the corn price, and the average temperature for each day.
Reward Scoring: I began with a simple scoring system that scaled directly with profit/loss, but it didn't have good results. Afterwards, I changed to a weighted system, where selling/holding above the previous buy price was good, selling/holding slightly below the buy price was bad but not terrible, and selling way below the buy price was terrible. This was to encourage the RL agent to play safe, minimizing losses rather than holding onto their losses.
### Step 3: Choosing an RL algorithm
Here, I was met with two choices. Either **PPO (Proximal Policy Optimization)** or **DQN (Deep Q-Network)**. Both are popular RL algorithms, but I decided to go with PPO, mainly because it was an on-policy algorithm, meaning it could learn with data collected during its current episode. Luckily enough, the **stable_baselines3** provided prebuilt functions that could take my OpenAI Gymnasium enviroment, and run it with the PPO algorithm.
### Step 4: Train!
Each episode (iteration) was 1000 days long. This was probably the step I spent the most time on. In the first round of iterations, there was 0 improvement. However, this was probably because my reward scoring wasn't the best--- instead of calculating short term rewards, I calculated long term gain/loss. Thus, if the agent made a bad trade early on, good trades that take place later may be deemed bad.

<img src="https://github.com/aletya/Corn-Trading-Reinforcement-Learning/assets/32620988/edd1d7f4-73f2-43e9-8f16-83397fc5e3c8" width="340" height="250">
Average gain/loss: -207.65

Next, I changed the reward function to better account for short term, as well as weight the losses to encourage cutting losses early:
- selling/holding above the previous buy price was good
- selling/holding slightly below the buy price was bad but not terrible
- selling way below the buy price was terrible. This was to encourage the RL agent to play safe, minimizing losses rather than holding onto their losses.
With this model, I saw a bit of improvement. I was plesantly surprised to see that the agent learned not to play so risky, which greatly reduced the standard deviation. However, it was definitely still very random. I think this was because I entirely neglected the overall gain/loss, which meant that even if the agent did terrible overall, if he ended on a good trade it was still marked as beneficial.

<img src="https://github.com/aletya/Corn-Trading-Reinforcement-Learning/assets/32620988/4945481a-ba05-4a1a-b8ec-848f7aa0bcc7" width="340" height="250">
Average gain/loss: -18.10


To account for this, I made the reward function a mix of both long and short term gain/loss. It took a bit of messing around with the multipliers for each reward component, but I settled for 1/3 to 1/2 long term, and 1/2 to 2/3 short term, depnding on the short term situational multiplier. Applying a least squares regression method, I got gain/loss = .053 * iteration - 32.9 with an r value of 18%, showing  evidence that the agent was finally learning!!

<img src="https://github.com/aletya/Corn-Trading-Reinforcement-Learning/assets/32620988/78ad5e0b-c8ba-448d-9721-5857dc5b460a" width="340" height="250">
Average gain/loss: -19.7

## Conclusion:
Overall, this was a pretty fun project. Pretty much everything was new to me, so it pretty interesting to go from watching videos on the general concept of reinforcement learning, to really getting a feel for it by seeing how exactly the computer "learned" from ground zero. Although the end result wasn't anything amazing, I'm still satisfied that I was able to get somewhat of a consistent improvement on my 3rd enviroment model. TLDR: Don't trust an AI to trade corn for you. It probably won't work.
