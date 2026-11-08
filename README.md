# Cookie Cats: gate_30 or gate_40

An A/B test on where to place the first gate in Cookie Cats. Players were assigned to a gate after 30 rounds or after 40 rounds. The question is whether moving the gate from 30 to 40 changes the chance that a player comes back.

The analysis is in `cookie_cats_ab.ipynb`. On retention, the evidence supports keeping the gate at 30.

## Result

Both return days were examined. The recommendation uses day 7, where the difference is clearer. That choice was made after seeing the results.

Out of every 1,000 players assigned to a gate, this many came back:

| Gate | Day 1 | Day 7 |
|---|---:|---:|
| `gate_30` | 448 | 190 |
| `gate_40` | 442 | 182 |

On day 1, `gate_30` has 6 more returning players per 1,000. On day 7, it has 8 more. The day-7 gap is small, and it favors `gate_30`.

The range below is where the true gap probably sits. The count of 8 can move by about 5 players either way, because these are samples.

| | Day 1 | Day 7 |
|---|---:|---:|
| Extra returning players per 1,000 in `gate_30` | 6 | 8 |
| Likely range | −1 to 12 | 3 to 13 |
| How often luck alone would make a gap this large | about 8 in 100 | about 2 in 1,000 |

On day 7 the range is 3 to 13 extra players per 1,000, all in favor of `gate_30`. Even the low end still shows more players coming back. Luck alone would create a gap this large about 2 times in 1,000, so the difference is likely real.

On day 1 the range runs from 1 player the other way to 12 extra players per 1,000. It includes no difference at all. Luck alone would create a gap this large about 8 times in 100, so the evidence there is weaker. The direction is the same as day 7.

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
4. **Conclusion.** On retention, the evidence supports keeping the gate at 30. On day 7, 190 of every 1,000 players came back in `gate_30`, and 182 in `gate_40`.

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
