## FunctionDef _compute_all_possible_actions
**_compute_all_possible_actions**: This function generates two dictionaries that map chess moves to actions and vice versa. It includes all possible standard moves (including castling) and promotion moves for a chess game.

parameters:
· None: The function does not accept any parameters.

Code Description: The function begins by initializing an empty list `all_moves` to store all possible moves in the form of strings representing the starting and ending squares of each move. It uses the `chess.BaseBoard.empty()` method to create an empty chessboard for analysis.

The function then iterates over every square on the board (from 0 to 63, where each number represents a unique position on the board). For each square, it places both a queen and a knight to determine all possible moves from that square. The queen's moves are used because they encompass the moves of rooks, bishops, and pawns, while the knight's moves cover the unique L-shaped jumps. These moves are added to `next_squares`, which is then appended to `all_moves` after converting each move into a string format representing the starting and ending squares.

After handling normal moves, the function addresses promotions, specifically for pawns reaching the opposite end of the board. It considers both regular promotions (where a pawn moves forward one square) and capture promotions (where a pawn captures an opponent's piece diagonally). For each promotion scenario, it generates possible moves that result in promoting to a queen ('q'), rook ('r'), bishop ('b'), or knight ('n'). These moves are added to `promotion_moves`, which is then combined with `all_moves`.

Finally, the function creates two dictionaries: `move_to_action` and `action_to_move`. Each move from `all_moves` is assigned a unique action number. The first dictionary maps each move string to its corresponding action number, while the second dictionary maps each action number back to its original move string. This bidirectional mapping facilitates easy conversion between moves and actions in chess algorithms.

Note: Usage points include scenarios where this function's output is necessary for encoding or decoding moves in a chess game, such as in reinforcement learning models that require numerical representations of moves.

Output Example: The function returns two dictionaries. Here is an example of what these might look like:
move_to_action = {'a1b2': 0, 'a1c3': 1, ..., 'h8g7q': 4679}
action_to_move = {0: 'a1b2', 1: 'a1c3', ..., 4679: 'h8g7q'}
## FunctionDef centipawns_to_win_probability(centipawns)
**centipawns_to_win_probability**: This function converts a chess score given in centipawns to an estimated win probability ranging from 0 to 1.

parameters:
· centipawns: The chess evaluation score expressed in centipawns, where positive values favor White and negative values favor Black.

Code Description: The function implements a well-known mathematical transformation that maps centipawn scores to probabilities. This transformation is based on empirical data collected from real-world chess games and provides an estimate of the likelihood of winning given a particular position's evaluation score in centipawns. The formula used is derived from logistic regression, specifically tailored for converting centipawn differences into win probabilities. It uses the mathematical constant e (Euler's number) to model the sigmoidal relationship between centipawn scores and win chances.

The function takes an integer input representing the centipawn score of a chess position. A positive value indicates that White is favored, while a negative value suggests Black has the advantage. The output is a float within the range [0, 1], where 0 represents no chance of winning (certain loss), and 1 signifies absolute certainty of winning.

The transformation formula used in this function is:
\[ P(\text{win}) = 0.5 + 0.5 \times \left(2 / (1 + e^{-0.00368208 \times \text{centipawns}}) - 1\right) \]
This formula ensures that as the centipawn score moves further from zero in either direction, the win probability approaches but never quite reaches 0 or 1, reflecting the inherent uncertainty and variability in chess outcomes.

Note: The conversion factor of 0.00368208 is a result of fitting the logistic curve to historical chess data, optimizing for accuracy in predicting game outcomes based on centipawn evaluations.

Output Example: If the function is called with an input of 100 centipawns (indicating White has a significant advantage), it might return a win probability close to 0.95, suggesting that White has a very high chance of winning from this position. Conversely, if the input is -200 centipawns, indicating Black's strong advantage, the function could output a win probability around 0.1, reflecting Black's higher likelihood of victory.
## FunctionDef get_uniform_buckets_edges_values(num_buckets)
**get_uniform_buckets_edges_values**: This function generates the edges and values of uniformly distributed buckets within the interval [0, 1]. It is particularly useful for tasks requiring equal partitioning of a range into specified intervals, such as histogram binning or data segmentation.

**parameters**:
· num_buckets: An integer specifying the number of buckets to create. The value must be positive to ensure meaningful bucket creation.

**Code Description**: The function starts by creating an array `full_linspace` that contains `num_buckets + 1` evenly spaced values between 0 and 1, inclusive. This array serves as a foundation for determining both the edges and the values of each bucket. 

The `edges` are derived from `full_linspace` by excluding the first and last elements, which represent the start (0) and end (1) of the interval [0, 1]. These excluded points define the boundaries between buckets.

The `values`, representing the midpoints of each bucket, are calculated by averaging consecutive pairs of values in `full_linspace`. This is achieved through slicing operations that exclude the last element for the first array and the first element for the second array, ensuring alignment of corresponding elements. The resulting averages correspond to the center points of each bucket.

**Note**: It's important to ensure that `num_buckets` is greater than zero to avoid errors in array indexing and logical inconsistencies in defining buckets.

**Output Example**: For a call with `num_buckets=4`, the function returns:
edges=[0.25, 0.50, 0.75]
values=[0.125, 0.375, 0.625, 0.875]

This output indicates that there are four buckets with edges at 0.25, 0.50, and 0.75, and the midpoints (or values) of these buckets are located at 0.125, 0.375, 0.625, and 0.875, respectively.
## FunctionDef compute_return_buckets_from_returns(returns, bins_edges)
**compute_return_buckets_from_returns**: This function arranges an array of discounted returns into specified bins defined by bin edges. It utilizes numpy's searchsorted method to determine which bucket each return value falls into, with ties resolved by placing the value in the bucket right before the edge.

parameters:
· returns: An array of discounted returns, expected to be one-dimensional (rank 1).
· bins_edges: The boundary values that define the edges of the buckets, also expected to be one-dimensional (rank 1).

Code Description: The function first checks if both `returns` and `bins_edges` are one-dimensional arrays. If not, it raises a ValueError indicating the incorrect rank. It then uses numpy's searchsorted method with the 'left' side parameter to find the indices of where each element in `returns` would fit into the sorted array `bins_edges`. These indices correspond to the bucket numbers for each return value.

Note: The function assumes that `bins_edges` is already sorted in ascending order. If this is not the case, the results will be incorrect. Additionally, if a return value exactly matches a bin edge, it will be placed in the bucket immediately preceding the edge due to the 'left' side parameter of searchsorted.

Output Example: Given `returns=[0., 1.]` and `bins_edges=[0.5]`, the function returns `[0, 1]`. This indicates that the first return value (0.) falls into the first bucket (before the bin edge), while the second return value (1.) falls into the second bucket (after the bin edge). For `returns=[-200., -30., 0., 1.]` and `bins_edges=[-30., 30.]`, the function returns `[0, 0, 1, 1]`, placing all values less than -30 in the first bucket (index 0) and all values greater than or equal to -30 but less than 30 in the second bucket (index 1).
