# Tiny Chess 60K

A chess engine in 4 KB of Python, written for the make.fun bounty
"Build the strongest chess engine in 4 KB of Python".

The idea behind this entry is simple: spend the whole move budget on
search, and make the search fast enough that a bigger budget still fits
the 10-second move limit. It runs a fixed 50,000-node search on every
move — about 2 seconds in a typical middlegame position — and never
looks at a clock, so a trial replayed from the same seed comes out move
for move identical. The budget was set at 50,000 after a practice trial
at 60,000 showed a slowest move of 9.21 seconds, too close to the limit
for comfort.

## How it plays

The engine talks to the trial environment, plays one game after another
against the house gauntlet, and answers every position with a move from
the fixed-budget search. No randomness, no model calls, no files read at
runtime, nothing stored outside its own bytes.

## The search

- 0x88 board with incremental material and piece-square evaluation, so
  the leaf score costs almost nothing.
- Zobrist transposition table with exact, lower-bound and upper-bound
  entries, plus killer moves and MVV-LVA capture ordering, so the best
  moves are tried first.
- Quiescence over captures and promotions, and over every reply when in
  check, so tactics are not missed at the horizon.
- Null-move pruning (two-ply reduction, only in safe positions) and a
  one-ply extension when the side to move is in check.
- Late-move reductions, both in the search and at the root: a quiet
  move tried late is first probed a ply shallower on a null window, and
  only re-searched at full depth when it looks good enough to matter.
- Iterative deepening over the 50,000-node budget; the best move of the
  last finished iteration is played.

## Practice trial

Practice trial-0 (6 games against the house gauntlet): Elo 1364 at
60,000 nodes — 3 wins, 1 draw, 2 losses, slowest move 9.21 s. At the
final 50,000-node budget the same trial scored Elo 1279 — 3 wins,
0 draws, 3 losses — with a slowest move of 2.60 s, comfortably inside
the 10-second limit.

## Files

- `main.py` — the whole engine, standard library only.
- `source.py` — the same engine in readable form; `main.py` is built
  from it with the kit's `compress` command.
- `LICENSE` — MIT.
- `.makefun/owner.json` — the claim file for this entry.

[![make.fun](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-5/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Rifat7600/tiny-chess-01-5/reward.svg)](https://make.fun/bounty/tiny-chess-01)
