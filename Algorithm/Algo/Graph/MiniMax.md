-   The Minimax algorithm is a recursive algorithm used for decision making in games with perfect information (i.e., games where all players have complete information about the state of the game).
-   It works by evaluating each possible move from the current state of the game, assuming that the opponent will choose the move that minimizes the player's score and the player will choose the move that maximizes their score.
-   The algorithm is based on the assumption that both players play optimally (i.e., they always choose the best possible move) and therefore it is guaranteed to find the optimal move for the player, assuming that the opponent plays optimally as well.
-   The algorithm works by building a game tree, where each node represents a possible state of the game and the edges represent the possible moves that can be made from that state.
-   The algorithm evaluates each node by recursively applying the Minimax algorithm to each child node and choosing the move that leads to the highest score for the player, assuming that the opponent will choose the move that leads to the lowest score for the player.
-   The algorithm assumes that the utility function (i.e., the function that assigns a score to each terminal state of the game) is known and can be evaluated for each leaf node in the game tree.
-   The Minimax algorithm can be optimized using techniques such as alpha-beta pruning, which can eliminate large parts of the game tree that are unlikely to lead to the optimal move.


---
### Tic-Tac-Toe

```python
# Define the utility function that returns the score for a given state of the game
def utility(state, player):
    # Return a positive score if the player has won, a negative score if the opponent has won,
    # and 0 if the game is a tie or has not ended yet.
    pass

# Define the Minimax function that returns the best move for a given state of the game
def minimax(state, player):
    # Check if the game has ended
    if is_terminal(state):
        return None, utility(state, player)
    
    # If it is the player's turn, maximize their score
    if player == MAX_PLAYER:
        best_score = -float('inf')
        for move in get_possible_moves(state):
            new_state = make_move(state, move, player)
            _, score = minimax(new_state, MIN_PLAYER)
            if score > best_score:
                best_score = score
                best_move = move
    
    # If it is the opponent's turn, minimize the player's score
    else:
        best_score = float('inf')
        for move in get_possible_moves(state):
            new_state = make_move(state, move, player)
            _, score = minimax(new_state, MAX_PLAYER)
            if score < best_score:
                best_score = score
                best_move = move
    
    return best_move, best_score
```