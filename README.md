# Chess Move Coach

A chess coach that runs entirely on your phone. Enter your opponent's move and it suggests your best reply and explains why. It can also review whole games and train you to find good moves yourself.

The chess engine is written from scratch in JavaScript, with no chess libraries.

## Features

- **Welcome screen:** continue your last game, or start a new one.
- **Play the coach:** play the computer at four strengths (Beginner, Casual, Club, Strong). The coach reacts like a human coach: a quiet ✓ for good moves, praise for strong finds, a short note when a slightly better move existed (with a **Why?** lesson on request), and it stops the game with a question when a move really costs you. Lessons run in labelled sections on the board: your move, what it allows, the better plan, and a takeaway. It also gives a heads-up when a tactic is waiting, and notices repeated habits.
- **Game helper:** enter your opponent's moves and get your best reply, with hint and training modes.
- **Who's winning bar** under the board, updated after every move.
- **Game review:** paste a game from Chess.com or Lichess for accuracy, a "who was winning" graph, and turning points. Tap any move for a step-by-step walkthrough showing why it was a mistake and what it cost.
- **Warnings** for pieces of yours that are under attack.
- **Load a position** from a FEN.
- **Works offline** as a Home Screen app, and remembers your game.

## How the engine works

| Part | What it does |
|---|---|
| Board | 0x88 board representation (128 squares), Zobrist hashing |
| Move generator | All legal moves incl. castling, en passant and promotion. Verified with perft on 5 standard test positions (e.g. 197,281 positions at depth 4 from the start). |
| Search | Iterative deepening alpha-beta with principal variation search, a transposition table, null-move pruning, late-move reductions, reverse futility pruning, check extensions, killer and history move ordering, and quiescence search |
| Evaluation | Tapered middlegame/endgame scores: material, piece-square tables, passed, doubled and isolated pawns, mobility, rooks on open files and the 7th rank, bishop pair, pawn shield and king attack |
| Opening book | About 35 main-line openings |
| Game review | Win% from the evaluation; each move rated by how much win% it loses (inaccuracy 5+, mistake 10+, blunder 15+ points). Accuracy uses the Lichess formula. |

## Measured strength

Each version played matches against **Stockfish 19** with its strength limited to a set rating (`UCI_LimitStrength`), at 0.3 seconds per move, from 8 different openings played once with each colour (16 games per match).

| Version | vs Stockfish 1700 | vs 2100 | vs 2400 | Estimated rating |
|---|---|---|---|---|
| v1 | 59% (9.5/16) | 28% (4.5/16) | n/a | about 1850 |
| v2 | n/a | 56% (9/16) | 63% (10/16) | about 2300 |

Head-to-head at 0.1 seconds per move, **v2 scored 90% against v1** (20 wins, 3 draws, 1 loss), about +370 rating points.

What made the difference (each variant played 24 games against v1):

| v2 variant | Score vs v1 |
|---|---|
| New search, simple v1 scoring | 73% |
| Everything, harsh king-safety penalty | 73% |
| Everything, no king safety | 85% then 75% (re-run) |
| **Everything, softer king safety (final)** | **90%** |

Lessons: the new search (transposition table and pruning) gave most of the gain, and a king-safety rule only helped once its penalty was made gentler.

Caveats: 16–24 games per match is a small sample, so each number is roughly ±150 points. The ratings are on Stockfish's own scale and are not the same as Chess.com ratings.

## Files

- `index.html`: the whole app (engine, interface, pieces)
- `manifest.webmanifest`: app name and icon for the Home Screen
- `sw.js`: offline support (bump the version name after changing `index.html`)
- `icon-*.png`: app icons

## Copyright

© 2026 Fazal Mahmood. All rights reserved.

Move Coach is made for learning chess. Using it for help during live games on chess sites breaks their fair-play rules; use it to practise and to review finished games. Not affiliated with Chess.com, Lichess, Apple or the Stockfish project.
