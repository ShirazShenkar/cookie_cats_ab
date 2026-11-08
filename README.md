# Cookie Cats: gate_30 or gate_40

An A/B test on where to place the first gate in Cookie Cats. Players were assigned to a gate after 30 rounds or after 40 rounds. The question is whether moving the gate from 30 to 40 changes the chance that a player comes back.

The analysis is in `cookie_cats_ab.ipynb`. On retention, the evidence supports keeping the gate at 30.

## Result

Both return days were examined. The recommendation uses day 7, where the difference is clearer. That choice was made after seeing the results.

Each row is one assigned gate. The percentage is the players who came back, divided by everyone assigned to that gate.

| Gate | Players | Came back on day 1 | Came back on day 7 |
|---|---:|---:|---:|
| `gate_30` | 44,700 | 20,034 (44.8%) | 8,502 (19.0%) |
| `gate_40` | 45,489 | 20,119 (44.2%) | 8,279 (18.2%) |

The same gap, counted as players per 1,000. `gate_30` minus `gate_40`.

| | Day 1 | Day 7 |
|---|---:|---:|
| Extra returning players per 1,000 in `gate_30` | 6 | 8 |
| How often luck alone would make a gap this large | about 8 in 100 (`p = 0.0755`) | about 2 in 1,000 (`p = 0.0016`) |

- **Extra returning players per 1,000 in `gate_30`.** This row is the gap we measured: how many more players came back in `gate_30`. On day 1 the gap is 6 players per 1,000. On day 7 it is 8. The day-7 gap is small, and it favors `gate_30`.
- **How often luck alone would make a gap this large.** If the gap were only luck, `p` would come out above 0.05. If it is not luck, and what happens in the game really supports a different choice, `p` should be below 0.05. On day 7 the notebook gives `p = 0.0016`, about 2 times in 1,000. That is below 0.05. In the game, that means players clearly prefer `gate_30`: more of them come back when the gate is after 30 rounds. On day 1, `p = 0.0755`, about 8 times in 100. That is above 0.05, so day 1 stays on the luck side of the line. The direction is still toward `gate_30`.

## Data

`cookie_cats.csv` has 90,189 players, one row per player.

| Column | Meaning |
|---|---|
| `userid` | Player id |
| `version` | Assigned gate: `gate_30` or `gate_40`. Assignment does not mean the player reached the gate. |
| `sum_gamerounds` | Rounds played during the measured period |
| `retention_1` | Came back the day after install |
| `retention_7` | Came back 7 days after install |

The split is close to even: 44,700 players in `gate_30` (49.6%) and 45,489 in `gate_40` (50.4%). The planned allocation is not in the file. If the plan was exactly 50/50, a sample-ratio check gives `p ≈ 0.0086`. The retention comparison still uses every assigned player.

## What the notebook covers

The notebook ends at the conclusion. The comparison uses every assigned player.

1. **Load and first look.** 90,189 players, one row each. `version` is the assigned gate, not proof that the player reached it.
2. **Data checks.** No missing values and no duplicate players. The split is close to half and half, and the planned ratio is unknown. Most players stop before either gate. One player in `gate_30` played 49,854 rounds; the decision uses retention, not the average number of rounds.
3. **Who came back.** Day-1 and day-7 return rates.
4. **The gap between the gates.** The calculation of the gap, the 95% margin, and p, then what those numbers mean.
5. **Conclusion.** On retention, the evidence supports keeping the gate at 30. On day 7, 190 of every 1,000 players came back in `gate_30`, and 182 in `gate_40`.

Still unmeasured:

- How many players reached the gate, how long they waited, and how much they paid to skip the wait.
- Purchases and revenue. Fewer returning players can still mean more money.
- Return counted from the moment a player reached the gate. `retention_1` and `retention_7` are counted from the install day.

## Run it

From this folder:

```bash
pip install pandas matplotlib scipy jupyter
jupyter notebook cookie_cats_ab.ipynb
```

The notebook reads `cookie_cats.csv` from the working directory.
