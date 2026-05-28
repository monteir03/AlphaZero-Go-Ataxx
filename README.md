# 🎮 Instructions to Run the Code

---

# 1. Game Configuration

All game settings are defined inside:

```python
configurations{}
```

This is where we choose:

* whether communication is enabled
* the game type
* the board size
* the game mode
* the delay between moves

---

# 2. Communication Mode

Communication can either be enabled or disabled.

## Disable Communication

```python
'communication': communication['off']
```

Use this mode when running locally without server-agent communication.

---

## Enable Communication

```python
'communication': communication['on']
```

Use this mode when running the server and agents through sockets.

---

# 3. Running With Communication

When communication is enabled, the game and board size are selected automatically from:

```python
'server_game': "G7x7"
```

Available options:

```python
"A4x4"
"A5x5"
"A6x6"
"G7x7"
"G9x9"
```

Where:

* `A` → Attax
* `G` → Go

Examples:

| Value  | Game  | Board Size |
| ------ | ----- | ---------- |
| `A4x4` | Attax | 4×4        |
| `A6x6` | Attax | 6×6        |
| `G7x7` | Go    | 7×7        |
| `G9x9` | Go    | 9×9        |

The program automatically extracts:

* the game type
* the board size

from the selected value.

---

## Important Note

When communication is enabled, the game mode does **not** need to be defined.

Only one mode is supported:

```python
agent_vs_agent
```

---

# 4. Running Without Communication

When communication is disabled, we must manually define the board size and game mode.

---

## Board Size

```python
'size': 7
```

Supported sizes:

| Size | Game  |
| ---- | ----- |
| `4`  | Attax |
| `5`  | Attax |
| `6`  | Attax |
| `7`  | Go    |
| `9`  | Go    |

---

## Game Mode

Example:

```python
'game_mode': game_mode['human_vs_human']
```

Available modes:

```python
game_mode['human_vs_human']
game_mode['agent_vs_agent']
game_mode['agent_vs_human']
```

---

# 5. Common Configuration

The following settings are required both with and without communication.

---

## Game Selection

```python
'game': play['Go']
```

or

```python
'game': play['Attax']
```

---

## Time Between Moves

```python
'time_between_moves': 100
```

Value is measured in **milliseconds**.

After a human move, the agent waits this amount of time before playing.

---

# 6. Board Definitions

The initial game boards are defined using NumPy arrays.

## Attax Boards

Defined in:

```python
board4{}
board5{}
board6{}
```

Supported sizes:

* 4×4
* 5×5
* 6×6

---

## Go Boards

Defined in:

```python
board7{}
board9{}
```

Supported sizes:

* 7×7
* 9×9

---

## Board Loading

Inside `main()`, the correct board is automatically selected according to the chosen board size.

---

# 7. Piece Representation

## Go

Go boards start completely empty.

```python
0 → empty position
```

---

## Attax

Attax boards may contain:

| Value | Meaning                   |
| ----- | ------------------------- |
| `0`   | Empty/free cell           |
| `3`   | Blocked/non-playable cell |
| `1`   | White piece               |
| `-1`  | Black piece               |

---

## Custom Initial Configurations

The initial piece placement can be modified directly in the NumPy arrays.

This allows testing different starting layouts instead of the traditional four-corner setup.

---

# 8. Communication Configuration

Communication parameters are defined in:

```python
communication{}
```

Parameters:

```python
'port'
'server_host'
'client_host'
```

---

## `server_host`

Use:

```python
"localhost"
```

when running on the same machine.

Use:

```python
""
```

when receiving connections from external machines.

---

## `client_host`

Use:

```python
"localhost"
```

when running locally.

Use:

```python
socket.gethostbyname(socket.gethostname())
```

when connecting to a server running on another machine.

---

# 9. Communication Workflow

If communication is enabled:

```python
communication['on'] == True
```

and:

```python
communication['off'] == False
```

then the execution order is:

1. Start the server
2. Start the first agent
3. Start the second agent

Each agent corresponds to one execution of:

```python
main()
```

---

# 10. main()

`main()` initializes the game using the parameters defined in:

```python
configurations{}
```

It also:

* loads the appropriate board
* initializes the selected game
* starts the game loop

---

# 11. Important Difference Between Attax and Go

The internal representation of players differs between the two games.

---

## Attax

The starting player is **white**.

| Value | Player |
| ----- | ------ |
| `1`   | White  |
| `-1`  | Black  |

---

## Go

The starting player is **black**.

| Value | Player |
| ----- | ------ |
| `1`   | Black  |
| `-1`  | White  |

---

# 12. Internal Communication Design

To simplify communication logic, the first player is always internally represented as:

```python
1
```

Therefore:

* in **Attax**, `1` corresponds to **white**
* in **Go**, `1` corresponds to **black**
