# CS470 Project Proposal

**Team 36**
| Student ID | Name         |
|------------|--------------|
| 20170058   | Keonwoo Kim  |
| 20160487   | Yongwook Lee |
| 20150723   | Junwon Jo    |

<p align="center">
<img src="https://dimg.donga.com/egc/CDB/WEEKLY/Article/15/08/20/27/1508202776070.jpg" />
</p>

## The Goal

Go-Stop is a popular card game in South Korea. It is played with a hanafuda deck, a deck of 48 cards grouped by 12 months, possibly with a number of additional cards (“bonus cards”). Typically there are two or three players, although there is a variation where four players can play. The objective of this game is to score a minimum predetermined number of points, usually three or seven, and then call a "Go" or a "Stop", where the name of the game derives. When a "Go" is called, the game continues, and the number of points or amount of money is first increased, and then doubled, tripled, quadrupled, and so on. A player calling "Go" risks another player scoring the minimum and winning all the points themselves. If a "Stop" is called, the game ends and the caller collects their winnings. 

The ultimate goal of this project is to make a model that shows better performance than an existing model, and have a higher winning rate and a higher expected income in games with humans and AI.

The random nature of the game is also an interesting point. Note that the games AlphaZero aimed to solve, such as Go, Chess, and Shogi, are all deterministic, meaning that there is no random factor during the game. Also, every information about such games is public to all players. However, it is not the case with Go-Stop. Hence, it is worthwhile to see if the network of AlphaZero works well with this game, which is highly random and of which the information is unbalanced.

## The Approach and Baselines

<p align="center">
<img src="https://miro.medium.com/max/2470/1*hQgJNK1LGYpXzFjBVxnREw.jpeg" />
</p>

Basically, we will use the reinforcement learning method. As a baseline, we may borrow the network structure of AlphaZero. Since AlphaZero aims to solve several general games, it may work to the Go-Stop game as well in a similar way. Furthermore, Go-Stop is a turn-based board game as well as Go, Chess, and Shogi.

As in AlphaGo, a previous version of Deepmind’s AI for Go, it uses the Monte Carlo Tree Search (MCTS.)  Using the MCTS, the agent could infer a fairly nicer move than a random one. However, a vanilla MCTS does not provide good exploitation but rather random exploitation. Therefore, it is difficult to choose a good move given a situation with this random exploitation.

To improve the MCTS, AlphaZero uses a method called PUCT algorithm, which is a revised version of the UCT algorithm. The UCT algorithm is a popular algorithm that supplements the MCTS algorithm, fixing the flaw described above.

To encode the board state into its associated policy and value, we will use a (possibly convolutional) residual neural network. Here, the policy means a probability distribution of the best next strategy, and the value represents the expected score from the board. Since a Go-Stop has no 2-dimensional geometric nature, it might not have to be convolutional, which may be different from the AlphaZero. After a few trials, we will decide whether the ResBlock becomes convolutional or not.

## Training
1. Encode information that the agent has.
  * The number of cards in the deck
  * List of cards discarded on the board
  * List of cards that the agent is holding
  * The number of cards that the opponent is holding
  * Lists of cards that the agent and its opponent have taken
  * Current points obtained by the agent and its opponent
  * Whether the agent should throw a card, flip a card from the deck (and the card flipped in this case), or choose among “Go” and “Stop.”
2. Translate the (encoded) board into a pair of policy and value with a deep residual neural network (as described in 2.)
3. Impose MCTS to explore a strategy and other algorithms to supplement the MCTS (as described in 2.)
4. Evaluate the neural network by playing either with a pre-made AI or self-playing.
   To gather the training data, we will collect training data by measuring performance from matches with known Go-Stop AI, or an own AI based on a simple heuristic. Then, if our model starts to show a good performance with the AI, we will make matches between duplicates of our models (self-play) to get train data.

## Evaluation

We will evaluate our model by two parameters; whether the model wins or not versus a predefined Go-Stop AI and itself, and the score of each game at the end. We want to make both the odds of win and the average score as high as possible, but the importance of the two parameters will not be the same since the object of Go-Stop is to gain a higher score, not winning all the games.

## Risk Management

In Go-Stop’s rule, each reward or penalty from the result of a single game is different since score calculation is based on player/opponent’s endpoints. This will make it harder to determine whether each movement done by the model in training is ‘good’ or not. We can give a guideline to the model by setting weights to a certain playstyle while the exploitation step, like ‘safety run with high odds but low average gain’ or ‘dangerous gambler seeking for highest prize,’ depending on what evaluation factor (discussed in 4) is chosen to be more important.

If our project doesn’t go well, first we will try to investigate the loss and find the problem. If we decide that the problem is intractable, we will consider changing the game the agent will solve to a rather simple game, like 2048-game and the snakes.

## References

1. https://en.wikipedia.org/wiki/Go-Stop
2. https://deepmind.com/blog/article/alphago-zero-starting-scratch 
3. https://openai.com/blog/competitive-self-play/ 

## Image Sources

1. https://dimg.donga.com/egc/CDB/WEEKLY/Article/15/08/20/27/1508202776070.jpg
2. https://miro.medium.com/max/2470/1*hQgJNK1LGYpXzFjBVxnREw.jpeg 
