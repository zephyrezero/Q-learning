# Q-Learning with Quantum Reward Modulation on FrozenLake

This project implements a Q-learning algorithm with quantum reward modulation to solve the FrozenLake-v1 environment from Gymnasium. Quantum circuit simulation is handled by Qiskit's AerSimulator (CPU backend), and Q-table operations use NumPy.

## Overview

- **Environment**: FrozenLake-v1 (4x4 grid, `is_slippery=False`), a deterministic gridworld where the agent navigates from start to goal, avoiding holes.
- **Algorithm**: Q-learning with an epsilon-greedy policy, augmented with quantum-modulated rewards.
- **Quantum Integration**: A single-qubit circuit generates a probabilistic reward modifier (0 to 1), simulated on CPU.
- **Computation**: Uses NumPy for Q-table operations and Qiskit’s AerSimulator for quantum simulation.

## Prerequisites

- **Hardware**: Standard CPU.
- **Software**:
  - Python 3.8 or higher
  - Required libraries:
    ```bash
    pip install gymnasium numpy qiskit qiskit-aer
    ```
- **Verify Installation**:
  ```python
  import qiskit
  print(qiskit.__qiskit_version__)
  ```

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   Create a `requirements.txt` file with:
   ```
   gymnasium
   numpy
   qiskit
   qiskit-aer
   ```

## Usage

1. Run the script:
   ```bash
   python q_learning_frozenlake.py
   ```
2. Expected output:
   - Training progress (epsilon values every 100 episodes).
   - Final Q-table.
   - Evaluation success rate (percentage of episodes reaching the goal).
   Example:
   ```
   Episode 100/500, Epsilon: 0.6058
   Episode 200/500, Epsilon: 0.3670
   ...
   Training finished.
   Q-table:
   [[0.65243678 0.80931064 0.26939791 0.60465628]
    ...]
   Evaluation: Success rate = 100.00%
   ```

## Algorithm Details

- **Q-Learning**:
  - Q-table: 16 states × 4 actions (left, down, right, up).
  - Parameters:
    - Learning rate: 0.1
    - Discount factor: 0.95
    - Epsilon: 1.0 (decays to 0.01 with decay rate 0.995)
  - Update rule: `Q(s,a) ← Q(s,a) + α [r + γ max(Q(s',a')) - Q(s,a)]`, where `r` is the modified reward.
- **Quantum Reward Modulation**:
  - Circuit: 1 qubit with random U-gate angles, measured to produce a probability `p0` (0 to 1).
  - Reward scaling: `modified_reward = reward * (1 + 0.1 * p0)`.
  - Simulated with AerSimulator (statevector method, 100 shots).
- **Training**: 500 episodes with epsilon-greedy exploration.
- **Evaluation**: 100 episodes using a greedy policy.

## Expected Results

- **Success Rate**: Approximately 100% for `is_slippery=False`, indicating an optimal policy in the deterministic environment.
- **Q-Table**: Non-zero values for state-action pairs along optimal paths, with high values (e.g., ~1.0) near the goal.

## Notes

- **Environment**: FrozenLake-v1 is simple, ideal for testing hybrid classical-quantum reinforcement learning. For stochastic behavior, set `is_slippery=True`.
- **Quantum Role**: The quantum circuit provides a minor reward perturbation, serving as a proof-of-concept for quantum integration.
- **Extensibility**: Adjust `episodes`, `learning_rate`, or quantum circuit complexity for further experiments.

