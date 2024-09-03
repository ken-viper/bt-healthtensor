<div align="center">

# HealthTensor

## Introduction

A Revolutionary Healthcare Domain Subnet on the Bittensor Network - HealthTensor

### How it works
At the core of our healthtensor subnet are the miners—highly skilled AI developers and teams dedicated to the continuous training and enhancement of machine learning models. These models are not static; they evolve through ongoing refinement. Miners publish their latest advancements on Hugging Face, a leading platform known for its extensive AI model repository, thereby extending the reach of these innovations to our community and beyond.

Validators, another integral part of our ecosystem, play a critical role in evaluating these models. They download the models from Hugging Face and rigorously assess them using their own diverse datasets. This evaluation process is complex, involving the ranking of models based on loss metrics. A lower loss indicates higher accuracy and efficiency, positioning the model favorably in its ability to address real-world challenges.

### Competition and Reward
Our subnet fosters a culture of healthy competition. Miners are incentivized not just to participate but to excel, as their rewards are directly tied to the performance of their models. The more accurate and efficient a model is, the greater the rewards for its creator. This competitive environment ensures that only the most effective models rise to prominence.

Validators are also rewarded for their indispensable role in the ecosystem. Through fair and unbiased evaluations, they uphold the network's transparency and meritocracy. Their assessments are crucial in determining which models receive the highest rewards, thereby influencing the direction of AI development on our subnet.
---
## Installation

### Install

#### Bittensor

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/opentensor/bittensor/master/scripts/install.sh)"
```

#### Clone the repository from Github
```bash
git clone https://github.com/0xdeity/bt-healthtensor.git
```

#### Install package dependencies for the repository
```bash
cd bt-healthtensor
apt install python3-pip -y
python3 -m pip install -e .
```

#### Install `pm2`
```bash
apt update && apt upgrade -y
apt install nodejs npm -y
npm i -g pm2
```

---
## Running

### Running subtensor locally

#### Install Docker
```bash
apt install docker.io -y
apt install docker-compose -y
```

#### Run Subtensor locally
```bash
git clone https://github.com/opentensor/subtensor.git
cd subtensor
docker-compose up --detach
```

### Running miner
In this cutting-edge healthtensor subnet, miners contribute significantly to disease diagnosis by developing predictive models based on medical images. Through continuous training and improvement, miners aim to enhance model accuracy, with higher-performing models earning substantial rewards. Miners have the flexibility to adjust and optimize the structure of their models, datasets, and other factors that influence accuracy. This collaborative effort is designed to advance disease prediction and highlights the essential role miners play in the future of medical diagnostics.

#### Download the dataset for model training
```bash
python3 healthtensor/dataset/downloader.py
```

#### Run the miner with `pm2`
```bash
# To run the miner
pm2 start neurons/miner.py --name miner --interpreter python3 -- 
    --netuid # the subnet netuid, default = 
    --subtensor.network # the bittensor chain endpoint, default = finney, local, test (highly recommend running subtensor locally)
    --wallet.name # your miner wallet, default = default
    --wallet.hotkey # your validator hotkey, default = default
    --logging.debug # run in debug mode, alternatively --logging.trace for trace mode
    --num_epochs # the number of training epochs (-1 is infinite), default = -1
    --batch_size # the number of data points processed in a single iteration, default = 32
    --save_model_period # the period of batches during which the model is saved, default = 30
    --model_type # the architecture and structure of the neural network used for training, default = CNN, VGG, RES, EFFICIENT, MOBILE
    --training_mode # the training mode, whether in fast, normal, or slow mode, dictates the pace and intensity of the model training process, default = normal
    --device gpu:0,2 # the device will be used for model training, default = gpu
    --restart # if set, miners will start the training from scratch, default = False
```

- Example 1 (with default values):
```bash
pm2 start neurons/miner.py --name miner --interpreter python3 -- --wallet.name default --wallet.hotkey default --logging.debug
```

- Example 2 (with custom values):
```bash
pm2 start neurons/miner.py --name miner --interpreter python3 --
    --subtensor.network local
    --wallet.name default
    --wallet.hotkey default
    --logging.debug
    --num_epochs 10
    --batch_size 256
    --restart True
    --model_type vgg
    --training_mode fast
    --device cpu:4
```

### Running validator
Validators further ensure the integrity of this process by periodically evaluating miners’ models with diverse images. They meticulously score these models based on performance, contributing to the continuous refinement of the models and maintaining the highest standards of accuracy and effectiveness within our collaborative network.
#### Run the validator
```bash
# To run the validator
pm2 start neurons/validator.py --name validator --interpreter python3 -- 
    --netuid # the subnet netuid, default = 
    --subtensor.network # the bittensor chain endpoint, default = finney, local, test (highly recommend running subtensor locally)
    --wallet.name # your miner wallet, default = default
    --wallet.hotkey # your validator hotkey, default = default
    --logging.debug # run in debug mode, alternatively --logging.trace for trace mode
    --vpermit_tao_limit # the maximum number of TAO allowed to query a validator with a vpermit, default = 4096
```

- Example 1 (with default values):
```bash
pm2 start neurons/validator.py --name validator --interpreter python3 -- --wallet.name default --wallet.hotkey default --logging.debug
```

- Example 2 (with custom values):
```bash
pm2 start neurons/validator.py --name validator --interpreter python3 --
    --subtensor.network local
    --wallet.name default
    --wallet.hotkey default
    --logging.debug
    --vpermit_tao_limit 1024
```