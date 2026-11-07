# Dry Run: poker bot submission

Team: **Dry Run** (pickle `dryrun.pkl`)

Submission for the MUDS AI Pokerbot Challenge (heads-up no-limit hold'em).
These files go flat next to the organisers' engine, as `python -m submit`
lists them:

| file | what it is |
|---|---|
| `dryrun.pkl` | `pickle.dump(Model())` from `model.py`, protocol 4 |
| `model.py` | the bot: `Model` with `seed`, `run`, `end_game` |
| `fasteval.py` | fast hand evaluator and Monte Carlo equity |
| `ranges.py` | starting-hand rankings from the preflop table |
| `preflop_table.py` | 169x169 preflop equity table (data only) |

No network, no files, no other imports beyond `random` and the engine's
`player_action`; the pickle names only `model.Model`, `random.Random` and
plain data. Checked with `python -m submit --team dryrun` and
`python -m pytest tests/test_model.py` (all pass).

## How it plays

It keeps per-opponent counts (by team name) of what they do in each betting
spot, turns those into a hand range, and plays the move with the most
expected chips against that range: preflop from the equity table, after the
flop by Monte Carlo. Old observations fade and showdowns correct the reads,
so it adapts to bots that change style or bet weak hands.
