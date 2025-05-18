# Benchmarking DreamerV3 vs SAC on FetchReach-v3

This repository presents a comparative study of **model-based** (DreamerV3) and **model-free** (SAC + HER) reinforcement learning approaches for robotic manipulation on the [`FetchReach-v3`](https://gymnasium.farama.org/environments/robotics/fetch/reach/) task from Gymnasium Robotics (MuJoCo).

## 🚀 Overview

Model-based RL (MBRL) is known for its **sample efficiency**, while model-free RL is often more **robust and generalizable**. This project benchmarks:

* 🌍 **DreamerV3**: A state-of-the-art model-based algorithm using latent world models and imagined rollouts.
* 🔁 **Soft Actor-Critic (SAC)** + **Hindsight Experience Replay (HER)**: A stable and widely-used model-free baseline.

We assess them on:

* Sparse and shaped reward settings
* Sample efficiency
* Goal-reaching success rate
* Training dynamics

## 📂 Project Structure

```
Fetch--Dreamer-Vs-SAC/

🔹 dreamer_fetch/           # DreamerV3 agent adapted to FetchReach-v3
🔹 ├── dreamer/             # World model, actor, critic, training logic
🔹 └── train.py             # Training script for DreamerV3

🔹 sac_fetch/               # SAC + HER setup
🔹 ├── train_sac.py         # Training script using SB3 with HER
🔹 └── env_wrapper.py       # Gymnasium environment wrapper

🔹 plots/                   # TensorBoard exports, reward/success plots
🔹 checkpoints/             # Optional: saved model checkpoints
🔹 DRL_Final_Report.pdf     # Full write-up of methods and results
```

## 🧠 Algorithms

### DreamerV3

* Latent imagination using Recurrent State-Space Model (RSSM)
* Training via imagined rollouts in latent space
* Goal-conditioning via custom feedforward encoder
* Custom HER implementation for real transitions

### SAC + HER

* Entropy-regularized stochastic policy
* Dual critic networks for stability
* SB3 implementation + future-goal HER relabeling
* Trained for 1M steps with dense reward shaping

## 🧪 Experiments

| Algorithm | Reward Type | Steps | Success Rate | Notes                                    |
| --------- | ----------- | ----- | ------------ | ---------------------------------------- |
| DreamerV3 | Dense       | 100k  | \~0–20%      | High sample efficiency, low final reward |
| SAC + HER | Dense + HER | 1M    | Up to 80%    | Slower start, more stable long-term      |

## 📈 Key Insights

* DreamerV3 converges faster and uses fewer real steps.
* SAC outperforms in **final success rate**, especially in sparse environments.
* Reward shaping and HER improve both agents.
* World model inaccuracies limit DreamerV3 on short-horizon tasks like FetchReach.

## ⚙️ Requirements

* Python 3.8+
* `torch`, `gymnasium`, `stable-baselines3`, `mujoco`, `tensorboard`
* GPU recommended (DreamerV3 runs efficiently on A100/T4)

Install dependencies:

```bash
pip install -r requirements.txt
```

## 🧪 Running the Code

### SAC

```bash
cd sac_fetch
python train_sac.py
```

### DreamerV3

```bash
cd dreamer_fetch
python train.py
```

## 📝 Reference

The detailed report describing methodology, training details, and discussion can be found here:

📄 [`DRL_Final_Project_SAC_Dreamer_Fetchv3_tushard.pdf`](./DRL_Final_Project_SAC_Dreamer_Fetchv3_tushard.pdf)

## 🙌 Acknowledgments

* [DreamerV3 Official](https://github.com/danijar/dreamerv3)
* [Stable-Baselines3](https://github.com/DLR-RM/stable-baselines3)
* Gymnasium Robotics + MuJoCo by [Farama Foundation](https://www.farama.org/)

---

Tushar Deshpande
Virginia Tech
[tsd10@vt.edu](mailto:tsd10@vt.edu)
