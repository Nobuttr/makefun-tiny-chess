[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Nobuttr/makefun-tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Nobuttr/makefun-tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# tiny-chess-01 — a 4 KB chess engine

`main.py` is a self-contained chess engine (Python 3, standard library only)
that plays through the make.fun agent/v1 environment protocol: it fetches the
current game from `MAKEFUN_ENV_URL`, computes a move, and posts it back,
validating every move against the environment's legal move list.

Design:

- 0x88 board representation with incremental evaluation: material plus my own
  piece-square tables (separate king tables for middlegame and endgame), and
  Zobrist hashing maintained incrementally across make/unmake.
- Negamax alpha-beta search with iterative deepening, a transposition table,
  quiescence search over captures/promotions and all check evasions (with
  delta pruning), null-move pruning, late-move reductions, check extensions,
  and mate-distance pruning.
- Move ordering: transposition-table move first, then MVV-LVA captures,
  killer moves, and a history heuristic.
- Draw awareness: repetition and the fifty-move rule score 0 inside the
  search, matching the house rules.
- Deterministic by construction: a fixed node budget per move
  (45k / 40k / 35k nodes depending on game ply), no wall-clock cutoffs and
  no randomness — every move is a function of the position and the game
  history only, so trials replay byte for byte.

The committed `main.py` is the lzma-compressed payload produced by the kit's
`makefun compress` from the engine source (4,091 bytes).
