## FunctionDef _play_game(engines, engines_names, white_name, initial_board)
**_play_game**: Plays a game of chess between two engines, starting from an optional initial board position. The function handles the turn-based gameplay, evaluates the board state using a stockfish engine, and determines the game's outcome based on the evaluation score.

parameters:
· engines: A tuple containing the two chess engines that will play against each other.
· engines_names: A tuple with the names of the engines corresponding to the order in the 'engines' parameter.
· white_name: The name of the engine that will play as White. This determines which engine makes the first move if the initial board is in its standard starting position.
· initial_board: An optional chess.Board object representing a non-standard starting position for the game. If not provided, the game starts from the standard chess setup.

Code Description: The function initializes the game by setting up the board and determining which engine plays as White based on the 'white_name' parameter. It then enters a loop that continues until the game is over due to checkmate, a fifty-move rule claim, or a threefold repetition. Within the loop, each engine takes turns making moves, with the move being selected by calling the 'play' method of the respective engine. After each move, the board's state is analyzed using a stockfish engine to evaluate the score. If the evaluation indicates a high score (either checkmate or a significant material advantage), the game ends early, and the result is determined based on whether the engine with the higher score was playing as White or Black. The function then creates a chess.pgn.Game object from the final board state, sets appropriate headers for the game such as event name, date, player names, and result, and returns this game object.

Note: This function is typically used within a larger context, such as a tournament where multiple games are played between different pairs of engines. It assumes that the engines provided have a 'play' method that takes a chess.Board object and returns a chess.Move object representing the engine's chosen move for the current position.

Output Example: A possible return value from this function could be a chess.pgn.Game object with headers set as follows:
- Event: UAIChess
- Date: 2023.10.05 (assuming today's date is October 5, 2023)
- White: Stockfish
- Black: LeelaZero
- Result: 1-0

The game object would also contain the moves played in the game and could be exported to a PGN file for record-keeping or analysis.
## FunctionDef _run_tournament(engines, opening_boards)
**_run_tournament**: This function orchestrates a chess tournament between multiple engines using specified opening board positions. It ensures each pair of engines plays two games for every opening, once with each engine playing as White.

parameters:
· engines: A dictionary mapping engine names to their respective engine objects. Each engine object must have a 'play' method that takes a chess.Board and returns a chess.Move.
· opening_boards: A list of chess.Board objects representing the starting positions (openings) for the games.

Code Description: The function initializes an empty list named `games` to store the results of each game. It then iterates over all unique pairs of engines using itertools.combinations, ensuring that every possible matchup is considered. For each pair of engines, it prints a message indicating which engines are playing against each other. 

For each matchup, the function further iterates over the list of opening boards and determines which engine plays as White by alternating between the two engines (using white_idx). It then calls the `_play_game` function to simulate a game between the selected engines starting from the given opening board. The initial board is copied using `copy.deepcopy` to prevent modifications in one game affecting others.

The result of each game, which is a chess.pgn.Game object containing details about the game such as moves and outcome, is appended to the `games` list. After all games have been played, the function returns the `games` list.

Note: This function assumes that the engines provided are capable of playing a game by implementing a 'play' method that takes a chess.Board and returns a chess.Move. The function also relies on the `_play_game` function to handle the actual gameplay mechanics.

Output Example: A possible return value from this function could be a list containing multiple chess.pgn.Game objects, each representing a game played between two engines during the tournament. Each game object would have headers set as follows:
- Event: UAIChess
- Date: 2023.10.05 (assuming today's date is October 5, 2023)
- White: Stockfish
- Black: LeelaZero
- Result: 1-0

The game objects would also contain the moves played in each game and could be exported to PGN files for record-keeping or analysis.
## FunctionDef main(argv)
**main**: The main function serves as the entry point for executing a chess tournament simulation between multiple engines using specified opening board positions from the Encyclopedia of Chess Openings (ECO). It handles command-line arguments, loads opening boards, initializes engines, runs the tournament, and saves the results to a PGN file.

parameters:
· argv: A sequence of strings representing the command-line arguments passed to the script. The function expects no additional arguments beyond the script name itself.

Code Description: The main function begins by checking if there are any command-line arguments provided besides the script name. If more than one argument is detected, it raises a UsageError indicating that too many command-line arguments have been supplied.

Next, the function constructs the path to the file containing the ECO openings in PGN format and initializes an empty list named `opening_boards` to store the board positions extracted from this file. It opens the file and reads each game using the chess.pgn.read_game method, appending the final board position of each game (obtained by calling the end() method on the game object) to the `opening_boards` list.

To ensure variability in the games played, the function uses NumPy's random number generator to randomly select a subset of opening boards. The size of this subset is determined by dividing the desired number of games (`_NUM_GAMES.value`) by two, as each selected opening will be used for two games (one with each engine playing White).

The function then initializes a dictionary named `engines` that maps engine names to their respective engine objects. These engines are created using factory functions stored in the `constants.ENGINE_BUILDERS` dictionary.

With the engines and opening boards prepared, the function calls `_run_tournament`, passing the engines and selected opening boards as arguments. This function orchestrates the tournament by having each pair of engines play two games per opening (one with each engine playing White).

Finally, the main function constructs a path to save the tournament results in PGN format and writes each game from the `games` list to this file. Each game is converted to a string using the str() method before being written to the file.

Note: The main function assumes that no additional command-line arguments are provided beyond the script name itself. It also relies on the `_run_tournament` function to handle the tournament logistics and the `_play_game` function (called within `_run_tournament`) to simulate individual games between engines. The results of the tournament are saved in a PGN file for further analysis or record-keeping.
