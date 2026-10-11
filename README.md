# Simulador-de-Ecosistemas

### Para iniciar el proyecto

Tomando en cuenta que la versión de Python es Python 3.12.1:

1. cd ecosystem_simulator (Raíz)
2. py -3.12 -m venv .venv (Si aún no se tiene un venv)
3. .venv/Scripts/activate
4. pip install -r requirements.txt
5. python run_with_godot.py

Enlace para instalar Python 3.12.1: https://www.python.org/downloads/release/python-3121/

# Ecosystem Simulator — Multi-Agent Reinforcement Learning + LLMs

Regional research project at Instituto Tecnológico de Costa Rica, supervised by a faculty member. Herbivore and predator agents learn to survive in a shared ecosystem through **multi-agent reinforcement learning**, while a **local Large Language Model** guides their behavior. The simulation is visualized in real time with **Godot**.

## Highlights
- **Custom multi-agent environment** built on the PettingZoo `ParallelEnv` interface: herbivores and predators compete for food, water and survival on a 2D map.
- **PPO training with Ray RLlib**, with a separate policy per species and optional **LSTM** (recurrent) policies so agents can remember past observations.
- **Three ways of combining RL with an LLM** (Ollama, running locally):
  1. **Behavior selector:** the LLM reads the agent's situation and chooses which pre-trained RL policy to activate.
  2. **Dynamic reward shaping:** the LLM analyzes each state and adjusts the reward function in real time (e.g. extra reward for fleeing when predators are near).
  3. **Reflexion-style learning:** inspired by the paper *Reflexion: Language Agents with Verbal Reinforcement Learning*, the LLM reviews complete episodes, writes "lessons learned" in natural language and feeds them into later episodes.
- **Real-time visualization:** a Flask API exposes the ecosystem state and a Godot 4 client renders it while the agents train.
- **Training monitor:** per-iteration metrics (mean reward, attack success, survival) are logged to `monitor.csv`.

## Results
After fixing reward-shaping exploits, hyperparameter issues and a seed reproducibility bug, the mean reward improved from **-104 to +761 over 900 training iterations**, showing genuine learning.

## Tech stack
Python · Ray RLlib (PPO) · PyTorch · PettingZoo · Gymnasium · SuperSuit · NumPy · SciPy · Pandas · Ollama (local LLM) · Flask · Godot 4

## Project structure
```
ecosystem_simulator/
├── environment/   # Multi-agent ecosystem (PettingZoo ParallelEnv)
├── entities/      # Species definitions
├── training/      # PPO trainer, LSTM policies and callbacks
├── llm/           # Behavior selector, reward shaping, Reflexion
├── server/        # Flask API consumed by Godot
└── utils/         # Visualization helpers
godot/             # Godot 4 visualization client
```

## Getting started
Requirements: [**Python 3.12**](https://www.python.org/downloads/release/python-3121/), [Ollama](https://ollama.com) running locally (e.g. `ollama run mistral`) and **Godot 4.5** for the visualization.

```bash
cd ecosystem_simulator                 # Root
2. py -3.12 -m venv .venv              # Create env
3. .venv/Scripts/activate
4. pip install -r requirements.txt
5. python run_with_godot.py            # To run with godot
```

Open `godot/simulador-ecosistemas` in Godot and run the scene to see the agents. Other entry points:

| Script | Description |
|--------|-------------|
| `run_with_llm_behavior_selector.py` | LLM selects between pre-trained policies |
| `run_with_llm_reward_shaping.py` | LLM adjusts rewards in real time |
| `run_with_llm_reflexion.py` | LLM learns from complete episodes |
| `examples_llm.py` | Test the LLM modules without Ray |

## Team
Jose Mario Jiménez Vargas ([@Zenstrok](https://github.com/Zenstrok)) and Fabricio Rios Montero, under faculty supervision.
