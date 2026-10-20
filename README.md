# CS-370-Project-Two-Pirate-Intelligent-Agent
# CS 370 Project Two: Pirate Intelligent Agent

**Mohannad Abuimeish** | CS 370 | Southern New Hampshire University

## About the Project

For this project I built the "brain" of a pirate in a treasure hunt game. The pirate starts on a random cell in an 8x8 maze and has to find the treasure in the bottom-right corner. It learns how to do this on its own using deep Q-learning, so nobody tells it where the walls are or what the best route is. It only gets rewards and penalties for what it does.

The notebook is `Abuimeish_Mohannad_ProjectTwo.ipynb` (an HTML export is included too).

## What I Was Given vs. What I Wrote

**Given to me:**
- `TreasureMaze.py`, which is the environment (the maze, the moves, the rewards, and the win/lose check)
- `GameExperience.py`, which stores past moves for experience replay and builds the training data
- The starter notebook with the maze, the `show()`, `play_game()`, and `completion_check()` helpers, the `build_model()` neural network, the `train_step()` helper, and the starting hyperparameters
- A skeleton of `qtrain()` with pseudocode telling me what to fill in

**What I wrote:**
- The full body of `qtrain()`, which is the actual deep Q-learning training loop:
  - Picking a random free cell to start each epoch
  - Epsilon-greedy action selection (random valid move vs. best predicted move)
  - Applying the move, tracking wins and losses, and storing each move in replay memory
  - Training the network on a random minibatch after every move
  - Syncing the target network every 50 steps
  - Decaying epsilon and stopping once the pirate hit a 100% win rate and passed `completion_check()`
- The training run and tuning. I trained with 1000 max epochs, a batch size of 32, and replay memory of 8 times the maze size. The pirate reached a 100% win rate at epoch 585 (about 10 minutes), passed the completion check from every free cell, and reached the treasure from the top-left corner.

One thing I ran into was that `get_data()` treats any non-empty value as "game over," so storing the status string `'not_over'` made every move look like the last one. I fixed it by storing a True/False flag instead.

## What Computer Scientists Do and Why It Matters

Computer scientists take real problems and figure out how to solve them with computation. Sometimes that means writing code, but a lot of the time it means choosing the right approach, testing whether it works, and explaining the tradeoffs. In this project, the problem was "how does a character find a path," and there were several possible answers. A search algorithm like A* could solve this one maze instantly, but reinforcement learning gave me an agent that learned from rewards and could find the treasure from any starting cell. That is the kind of decision computer scientists make all the time.

It matters because software now sits behind almost everything people rely on, like games, banking, healthcare, and transportation. The same ideas I used here, learning from feedback and improving over time, are used in robotics, recommendations, and self-driving systems. Getting these things right affects real people.

## How I Approach a Problem

I start by making sure I understand the problem and what a good result looks like. Here that meant reading the starter code and figuring out how the maze, rewards, and replay memory all fit together before writing anything. Then I break it into smaller pieces. I built `qtrain()` one part at a time (action selection, then storing experience, then training) instead of trying to write it all at once.

After that I test and adjust. The win rate and loss output told me whether the agent was actually learning, and the bug with the game-over flag showed me that I need to check my assumptions about how other code behaves instead of just trusting my own. I also try to remember that a good number isn't always proof. A 100% win rate can just mean the pirate got lucky in the last few games, which is why I used `completion_check()` to confirm it worked from every cell.

## My Ethical Responsibilities to the End User and the Organization

To the **end user**, I'm responsible for building something that does what it says and doesn't cause harm. In a game, a bad agent just means a less fun experience, but the same techniques are used in systems that make decisions about people. If I build something like that, I need to test it carefully, be honest about its limits, and think about who could be hurt when it gets something wrong. I also need to respect people's data and privacy, and not collect or use more than I need.

To the **organization**, I'm responsible for being honest about my work and delivering quality I can stand behind. That means not overstating results, giving credit for code and ideas that aren't mine (like the starter classes in this project), documenting my work so others can understand it, and speaking up if I see a problem or a shortcut that could cause trouble later. The organization is trusting me to do the job right, and the users are trusting the organization, so both depend on me being responsible.
