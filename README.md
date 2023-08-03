# Corn Reinforcement Learning
Alex Yang
## Overview
- Teaching an AI to trade corn from ground zero, based on corn prices and weather.
- Using historical data, I trained a custom reinforcement learning (RL) agent to trade corn for profit.
- Language: **Python**
- Libraries: **OpenAI Gymnasium**, **stable_baselines3**, **pandas**, **numpy**, **matplotlib**
- Google Collabs Link: https://colab.research.google.com/drive/1hzc0-o3NRSmvXSABePQ1KiwJzx-0C2V3?usp=sharing
## Motivation
I saw a prety cool video of an AI learning to walk from ground zero, and wanted to try to replicate trading in a similar way. I wondered if an AI could pick up trends between weather and corn prices, and use it to make smart trades.
## Process
### Step 1: Gathering and formating data
For corn prices, I used **Alpha Vantage**, a free API that provided historical financial market data. Luckily, they had the option to request the data in a pandas database format. As for weather data, I used **Visual Crossing Weather**. The data came in a CSV format, so I converted it to a pandas database. I merged the two based on matching dates, and kept only the price and temperature columns for each day. Finally, I **normalized** the data using a standard **Z-score** scale, an essential step in the RL process to ensure that stability and convergence(large outlier may lead to unstable updates during training), as well as compatibility with the RL algorithm.
### Step 2: Creating a custom RL enviroment
I created a cutom RL enviroment based on the the OpenAI Gymnasium interface, which is a standard library provided by OpenAI that allows one to interact with various (RL) environments in a consistent and unified way. In my case, it was really helpful since it took care of all the behind the scenes processing, and let me focus on the code that would shape the RL agent's enviroment--- namely, their actions, their observation space, and their reward scoring.
### Step 3: 

### Step 4: Train!
