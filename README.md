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

`gate_30` minus `gate_40`:

| | Day 1 | Day 7 |
|---|---:|---:|
| Difference | 0.6 percentage points | 0.8 percentage points |
| 95% confidence interval | −0.1 to 1.2 | 0.3 to 1.3 |
| p | 0.0755 | 0.0016 |

The difference is the subtraction of the two rates. On day 7, 19.0% − 18.2% = 0.8 percentage points, about 8 extra returning players out of every 1,000, in favor of `gate_30`. The gap is still small.

The 95% confidence interval is the range of differences that fits this sample. On day 7 it stays above zero, from 0.3 to 1.3. On day 1 it runs from −0.1 to 1.2 and includes no difference, so the evidence there is weaker.

`p` asks how often a gap at least this large would show up by chance if the gate made no difference. Day 7 is `p = 0.0016`, about 2 experiments out of 1,000. Day 1 is `p = 0.0755`.

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
4. **Conclusion.** On retention, the evidence supports keeping the gate at 30. Day 7 return is 19.0% in `gate_30` and 18.2% in `gate_40`.

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
