# Instructions to run the code

> configurações do jogo

## In configurations{}, we choose what we want.

#### We can choose either if we want communication or not

'comunication': comunication['off'] -> if we don't want communication

'comunication': comunication['on'] -> if we want communication


#### With communication

We indicate the server what game we are playing and the board size.

'server_game': "G7x7"  ("A4x4", "A5x5", "A6x6", "G7x7", "G9x9")

From this, we will extract the size game.


#### Without communication

We provide the size game: 'size': 7 (7/9 -> go, 4/5/6 -> attax)

We provide the game mode: 'game_mode': game_mode['human_vs_human'] (or['agent_vs_agent']or['agent_vs_human'])

!! Note that for the communication way, we don't need the game mode, as there's only one game mode defined: agent_vs_agent !!


#### With and without communication

We provide the type of game: 'jogo': jogar['Go'] or 'jogo': jogar['Attax'] 

We provide the time between moves: 'time_between_moves': 100 (the agent, after our human move, takes "time_between_moves" miliseconds to play)


## In board4{}, board5{}, board6{} (Attax) and in board7{}, board9{} (Go) we define the numpy arrays based on each size

This numpy array is then called by main() based on the size it receive, representing the game's initial state.

To Attax, we have numpy arrays 4 by 4, 5 by 5 and 6 by 6.

To Go, we have numpy arrays 7 by 7 and 9 by 9.

The free pieces are known as 0 (to Go, it is all 0's).

!! The following only occurs in Attax !!

The occupied pieces can be of 2 types: non playable blocks, known as 3; playable blocks but initial occupied by game pieces, known as -1 (if black piece) or 1 (if white piece).

We also can try distinct initial configurations instead of the typical 4 corner pieces start.

As the numpy array is defined by a matrix, we can only change the location of the pieces in configurações do jogo.


## In comunication{} we define the communication parameters

'port'
'server_host' -> "= localhost" if we are running in our own machine; " = '' " if we are receiving from other machines
'client_host' -> "= localhost" if we are running in our own machine; our machine host ("= socket.gethostbyname(socket.gethostname())") if we are sending to the server in other machine

We also have the 'on' and 'off' parameters and, if 'on' is True and 'off' is False, we first run the server and then the two agents (each one represented by main()).

> main()

In main(), we inicialize our game with the instructions provided on configurações do jogo.

> class Attax and class Go

There's an important notion we need to have.

In Attax, the starting player is white, so white is known as 1 and black is known as -1.

However, in Go, the starting plater is black, so white is known as -1 and black is known as 1.

This help in communication, as the first agent to play is always 1 (white in Attax and black in Go).