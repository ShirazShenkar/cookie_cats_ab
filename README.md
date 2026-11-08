# Cookie Cats: gate_30 or gate_40

An A/B test on where to place the first gate in Cookie Cats. Players were assigned to a gate after 30 rounds or after 40 rounds. The question is whether moving the gate from 30 to 40 changes the chance that a player comes back.

The analysis is in `cookie_cats_ab.ipynb`. On retention, the evidence supports keeping the gate at 30.

## Result

Both return days were examined. The recommendation uses day 7, where the difference is clearer. That choice was made after seeing the results.

| | gate_30 | gate_40 | Difference | 95% CI | p |
|---|---:|---:|---:|---:|---:|
| Day 1 return | 44.8% | 44.2% | 0.6 pp | -0.1 to 1.2 | 0.0755 |
| Day 7 return | 19.0% | 18.2% | 0.8 pp | 0.3 to 1.3 | 0.0016 |

The day-7 gap is about 8 extra returning players out of every 1,000, in favor of `gate_30`. The interval stays above zero. The gap is still small. Day 1 points the same way, and the evidence there is weaker: the interval includes no difference.

The file has no purchases or revenue. If the business goal is money, retention alone cannot decide the move.

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
3. **Retention by assigned version.** Day-1 and day-7 return rates, the difference in percentage points, a 95% confidence interval, and a chi-square p-value.
4. **Conclusion.** On retention, the evidence supports keeping the gate at 30. One hypothesis is left open: an earlier stop may leave players wanting to come back. The file cannot show why players returned.

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
