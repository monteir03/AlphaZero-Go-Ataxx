# 🎮 AlphaZero Go & Ataxx

A comprehensive Python implementation of two classic board games (**Go** and **Ataxx**) featuring intelligent AI agents, a graphical user interface, and socket-based networked play.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Stack](#stack)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Game Modes](#game-modes)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Game Rules & Board Setup](#game-rules--board-setup)
- [Communication Protocol](#communication-protocol)
- [Advanced Usage](#advanced-usage)

---

## 🎯 Overview

This project implements two classic board games with AI capabilities:

- **Ataxx**: A placement-and-infection strategy game on 4×4, 5×5, or 6×6 boards
- **Go**: The ancient board game of strategy on 7×7 or 9×9 boards

The system supports multiple play modes (human vs. human, human vs. AI, AI vs. AI), implements the **AlphaZero-inspired** Monte Carlo Tree Search (MCTS) algorithm with neural network integration, and provides both local and networked gameplay via socket communication.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| **Multiple Game Modes** | Human vs. Human, Human vs. AI, AI vs. AI |
| **Networked Play** | Socket-based communication for distributed agent competition |
| **AI Engine** | Monte Carlo Tree Search (MCTS) with PyTorch neural network support |
| **GUI Interface** | Tkinter-based graphical board visualization |
| **Configurable Rules** | Threefold repetition (Ataxx), Ko rule (Go), komi scoring (Go) |
| **Custom Boards** | Easily define custom starting positions with blocked cells (Ataxx) |
| **Flexible Configuration** | JSON-like Python dictionary configuration system |

---

## 🛠️ Stack

| Component | Technology |
|-----------|-----------|
| **Language** | Python 3.x |
| **GUI Framework** | Tkinter |
| **AI/ML** | PyTorch, NumPy |
| **Networking** | Python `socket` module |
| **Notebook Environment** | Jupyter |
| **Dependencies** | `torch`, `tqdm`, `numpy`, `ipywidgets` |

---

## 📦 Installation

### Prerequisites

- Python 3.7+
- pip

### Setup

```bash
# Clone the repository
git clone https://github.com/monteir03/AlphaZero-Go-Ataxx.git
cd AlphaZero-Go-Ataxx

# Install dependencies
pip install torch numpy tqdm ipywidgets

# For GUI (Tkinter)
# On Ubuntu/Debian:
# sudo apt-get install python3-tk
# On macOS:
# brew install python-tk
# On Windows: Tkinter is included with Python
```

---

## 🚀 Quick Start

### 1. Local Play (Offline)

Edit the configuration in `trabalho.ipynb` and set:

```python
configurations = {
    'comunication': comunication['off'],  # Disable networking
    'game_mode': game_mode['agent_vs_agent'],  # or 'human_vs_human', 'agent_vs_human'
    'jogo': jogar['Ataxx'],  # or 'Go'
    'size': 4,  # 4, 5, 6 for Ataxx; 7, 9 for Go
    'time_between_moves': 100,  # milliseconds
}
```

Then run the notebook's `main()` cell.

### 2. Networked Play

**Server setup:**
```python
# Start the server first (from server.py or a dedicated cell)
```

**Agent 1 (Client 1):**
```python
configurations = {
    'comunication': comunication['on'],
    'server_game': "A4x4",  # "A4x4", "A5x5", "A6x6", "G7x7", "G9x9"
    'port': 12401,
    'server_host': 'localhost',
    'client_host': 'localhost',
}
main()
```

**Agent 2 (Client 2):**
```python
# Same as Agent 1, runs in a separate Jupyter kernel or environment
main()
```

---

## 🎮 Game Modes

### Mode 1: Human vs. Human (Offline)
```python
'game_mode': game_mode['human_vs_human']
```
Click to select a piece, click again to move it.

### Mode 2: Human vs. Agent (Offline)
```python
'game_mode': game_mode['agent_vs_human']
```
Human plays white, agent plays black (for Ataxx) or vice versa.

### Mode 3: Agent vs. Agent (Offline)
```python
'game_mode': game_mode['agent_vs_agent']
```
Watch two AI agents play against each other.

### Mode 4: Networked Agent vs. Agent
```python
'comunication': comunication['on']
```
Two agents connect via sockets and play on a shared server state.

---

## ⚙️ Configuration Guide

### Game Selection

```python
jogar = {
    'Ataxx': 'A',
    'Go': 'G'
}

configurations = {
    'jogo': jogar['Ataxx']  # or jogar['Go']
}
```

### Board Size

| Size | Game  | Notes |
|------|-------|-------|
| 4    | Ataxx | Smallest, fastest |
| 5    | Ataxx | Medium |
| 6    | Ataxx | Largest Ataxx board |
| 7    | Go    | Small |
| 9    | Go    | Standard 9×9 Go |

### With Communication Enabled

When `'comunication': True`, boards are auto-selected from `server_game`:

```python
'server_game': "G7x7"  # Format: <GAME><SIZExSIZE>

# Available options:
# "A4x4", "A5x5", "A6x6"  (Ataxx boards)
# "G7x7", "G9x9"          (Go boards)
```

The game type and size are **automatically extracted** from this string.

### Without Communication (Offline)

```python
configurations = {
    'comunication': comunication['off'],
    'size': 6,  # Explicit board size
    'game_mode': game_mode['human_vs_human'],  # Explicit mode
    'jogo': jogar['Ataxx'],  # Explicit game type
}
```

### Timing

```python
'time_between_moves': 100  # milliseconds (only used offline)
```

---

## 🏗️ Architecture

### Class Hierarchy

```
Ataxx
  - State management (4×4, 5×5, 6×6 boards)
  - Move generation (radius 1 & 2 placement rules)
  - Infection/piece flipping
  - Threefold repetition detection
  - Neural network encoding (3-channel state)
  - Action space management

Go
  - State management (7×7, 9×9 boards)
  - Liberty & group detection (DFS-based)
  - Stone capture rules
  - Ko rule enforcement
  - Scoring with komi
  - Pass move support

interfaceAtaxx
  - Tkinter GUI rendering
  - Event binding (mouse clicks)
  - Turn management
  - Communication interface (socket send/recv)

Node (MCTS)
  - Tree node for Monte Carlo Tree Search
  - UCB selection strategy
  - Child expansion
  - Backpropagation

AlphaZero
  - MCTS + neural network integration
  - Self-play training
  - Policy and value networks
```

### Data Flow

1. **Configuration** → Game initialization
2. **Game Loop** → State → Agent decision → Move validation → State update → Interface render
3. **AI Decisions** → MCTS exploration → NN evaluation → Best move selection
4. **Networking** → Move encoding → Socket transmission → Opponent receives → State sync

---

## ♟️ Game Rules & Board Setup

### Ataxx

**Board Representation:**
```python
1  = White piece
-1 = Black piece
0  = Empty cell
3  = Blocked/non-playable cell
```

**Initial Setup:**
```python
board6 = {
    'size': 6,
    'board': np.array([[1, 0, 0, 0, 0, -1],    # White (1) corners
                       [0, 0, 0, 0, 0, 0],
                       [0, 0, 0, 3, 0, 0],      # Blocked cells (3)
                       [0, 0, 3, 0, 0, 0],
                       [0, 0, 0, 0, 0, 0],
                       [-1, 0, 0, 0, 0, 1]])    # Black (-1) corners
}
```

**Rules:**
- Place new piece up to **2 squares away** (radius 2)
- Infect adjacent enemy pieces (radius 1) after placement
- If move distance = 2 (jump), remove original piece
- Game ends when board is full, player is stuck, or **threefold repetition occurs**

**Threefold Repetition:**
Automatic draw if the same board state occurs 3 times in the last 9 moves.

### Go

**Board Representation:**
```python
1  = Black stone (player 1 starts)
-1 = White stone (player -1 plays second)
0  = Empty point
```

**Starting Position:**
```python
board7 = {
    'size': 7,
    'board': np.zeros((7, 7), dtype=int)  # Empty board
}
```

**Rules:**
- Place a stone on any empty intersection
- Capture opponent groups with no liberties (empty adjacent points)
- **Ko rule**: Cannot recreate the board state from the immediately previous move
- Pass move: `(-1, -1)` passes the turn (2 consecutive passes = game end)

**Scoring (Chinese Rules):**
- Count territory: empty points surrounded by one color
- Add captured stones to that color's score
- White receives **komi = 5.5** (compensation for playing second)

**Internal Representation:**
First player (1) is always Black; second player (-1) is White.

---

## 🔌 Communication Protocol

### Socket Communication

**Port:** Configurable (default: `12401`)

**Message Format for Ataxx:**
```
Ataxx moves encoded as: "x,y,movex,movey"
Example: "0,0,1,1" means pick (0,0), move to (1,1)
```

**Message Format for Go:**
```
Go moves encoded as: "x,y" (coordinate placement) or "-1,-1" (pass)
```

**Handshake:**
1. Server receives first connection (Agent 1)
2. Server receives second connection (Agent 2)
3. Server broadcasts game state and waits for moves
4. Moves are validated, state updated, transmitted to opponent

**End Signal:**
```
"END"  # Signals game termination
```

---

## 🧠 Advanced Usage

### Custom Initial Boards

Edit NumPy arrays in configuration:

```python
board4 = {
    'size': 4,
    'board': np.array([
        [1, 0, 3, -1],     # Row 0: White, empty, blocked, Black
        [0, 0, 0, 0],      # Row 1
        [0, 0, 0, 0],      # Row 2
        [-1, 3, 0, 1]      # Row 3: Black, blocked, empty, White
    ])
}
```

Any configuration can be tested by modifying these arrays.

### MCTS Integration

The `Node` class implements MCTS with:
- **UCB (Upper Confidence Bound)** for child selection
- **Prior probability** from neural network
- **Visit counts** for exploitation vs. exploration

```python
class Node:
    def __init__(self, game, args, state, parent=None, action_taken=None, prior=0):
        self.prior = prior
        self.children = []
        self.visit_count = 0
        self.value_sum = 0
    
    def select(self):
        # Select best child by UCB
        best_child = max(self.children, key=lambda c: self.get_ucb(c))
        return best_child
```

### Neural Network Encoding

**Ataxx:**
```python
encoded_state = np.stack(
    (state == -1, state == 0, state == 1)  # 3 channels
).astype(np.float32)
# Shape: (3, size, size)
```

**Go:**
Similar 3-channel representation with liberty information.

---

## 📝 Instructions by Configuration

### Running Human vs. Human (Ataxx, Offline)

```python
configurations = {
    'comunication': comunication['off'],
    'jogo': jogar['Ataxx'],
    'size': 4,
    'game_mode': game_mode['human_vs_human'],
}
main()
```

### Running AI vs. AI (Go, Offline)

```python
configurations = {
    'comunication': comunication['off'],
    'jogo': jogar['Go'],
    'size': 7,
    'game_mode': game_mode['agent_vs_agent'],
}
main()
```

### Running Networked Agents

**Terminal 1 (Server):**
```python
# Run server.py or execute server initialization
```

**Terminal 2 (Agent 1):**
```python
configurations = {
    'comunication': comunication['on'],
    'server_game': "G7x7",
    'port': 12401,
    'server_host': 'localhost',
}
main()
```

**Terminal 3 (Agent 2):**
```python
# Same as Agent 1, connects to same server
main()
```

---

## 🎓 Key Differences: Ataxx vs. Go

| Feature | Ataxx | Go |
|---------|-------|-----|
| **Starting Board** | Pre-populated | Empty |
| **Piece Placement** | Radius 1 & 2 | Any empty intersection |
| **Capture Mechanic** | Infection (automatic) | Liberties (explicit groups) |
| **Rule Complexity** | Medium | Complex (Ko, liberties, groups) |
| **Termination** | Full board / Stuck / 3-fold repetition | Pass-pass / Agreed end |
| **Board Size** | 4×4, 5×5, 6×6 | 7×7, 9×9 |

---

## 🐛 Troubleshooting

### Issue: `"Invalid move"` message repeatedly appears
**Solution:** Ensure the game mode matches your input method. For AI-only modes, the interface will auto-play.

### Issue: Socket connection refused
**Solution:** 
1. Ensure server is running first
2. Check `server_host` and `client_host` are correct
3. Verify firewall allows port 12401

### Issue: Game doesn't start
**Solution:** 
1. Check all required dependencies are installed
2. Ensure configuration keys match exactly
3. Run from Jupyter with all configuration cells executed

---

## 📚 References

- **Go Rules:** [American Go Association](https://www.usgo.org/)
- **Ataxx Rules:** [BoardGameGeek](https://boardgamegeek.com/boardgame/388/ataxx)
- **AlphaZero:** Schrittwieser et al., arXiv:1902.01724
- **PyTorch:** https://pytorch.org/docs/stable/index.html

---

## 📄 License

This project is open-source. See LICENSE file for details.

---

## 👤 Author

**monteir03** - [GitHub Profile](https://github.com/monteir03)

---

## 📞 Support & Contribution

For issues, feature requests, or contributions, please open an issue or submit a pull request on GitHub.

---

**Last Updated:** 2024 | **Status:** Active Development
