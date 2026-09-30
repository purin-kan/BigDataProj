# dToU tariff continuity

Presentation feedback: mind the continuity of the dToU tariff. Does the price level switch, and how long do the usual stretches (for example Low) last without a break?

Source: `Tariffs.xlsx` (London Datastore, resource `vqm0d`), loaded in `src/01_eda_smart_meters.ipynb` Section 0b. 17,520 rows, one `Tariff` label (High / Normal / Low) per half-hour slot, 2013-01-01 00:00 to 2013-12-31 23:30. Cached file dated 2026-09-29 13:18; notebook last committed 2026-09-29 16:27. Timestamps used as given in the file.

Method: a run is a maximal stretch of consecutive slots with the same label. Slots are half-hours, so 2 slots = 1 hour.

```python
s = pd.read_excel(".../Tariffs.xlsx").sort_values("TariffDateTime")
run = (s.Tariff != s.Tariff.shift()).cumsum()
r = s.groupby(run).agg(level=("Tariff", "first"), start=("TariffDateTime", "min"), n=("Tariff", "size"))
```

## Does it switch

Yes. 272 runs, so 271 label changes across the year. Slot counts: Normal 15,072 (86.0%), Low 1,660 (9.5%), High 788 (4.5%).

Most switches go through Normal, but 51 go straight between the two event levels:

| From | To | Count |
|---|---|---|
| Normal | High | 44 |
| Normal | Low | 66 |
| High | Normal | 43 |
| Low | Normal | 67 |
| High | Low | 26 |
| Low | High | 25 |

## How long a stretch lasts

| Level | Runs | Min | Median | Mean | Max |
|---|---|---|---|---|---|
| Low | 92 | 4 slots (2 h) | 12 slots (6 h) | 18.04 slots (9.0 h) | 60 slots (30 h) |
| High | 69 | 6 slots (3 h) | 12 slots (6 h) | 11.42 slots (5.7 h) | 48 slots (24 h) |
| Normal | 111 | 6 slots (3 h) | 120 slots (60 h) | 135.78 slots (67.9 h) | 552 slots (276 h) |

Slot-length counts for the event levels (slots: runs):

- Low: 4: 2, 6: 24, 10: 1, 12: 26, 24: 25, 30: 1, 36: 6, 38: 2, 48: 3, 60: 2
- High: 6: 26, 8: 1, 12: 34, 24: 7, 48: 1

Event runs come in multiples of 6 slots (3 h) in 87 of 92 Low runs and 68 of 69 High runs. The exceptions are Low at 4, 10 and 38 slots and High at 8 slots.

The longest Normal stretch is 552 slots, 2013-09-18 05:00 to 2013-09-29 16:30.

## Shortest stretches

The shortest run of any level is 4 slots (2 h), and only Low reaches it.

- **Low, 4 slots (2 runs):** 2013-01-29 05:00 to 06:30 and 2013-03-21 05:00 to 06:30. Both sit between Normal and High. Each day repeats the same pattern: Low 4 slots, High 6 slots (07:00 to 09:30), then Low 38 slots (10:00 to 04:30 the next day).
- **Low, 6 slots (24 runs):** the next shortest, 3 h each. Example: 2013-01-04 14:00 to 16:30 (the first event of the year).
- **High, 6 slots (26 runs):** 3 h each, the minimum for High. 23 of the 26 sit between two Normal stretches. 2 (2013-01-29 07:00 and 2013-03-21 07:00) sit between two Low blocks, the High in the pattern above. 1 (2013-10-31 02:00) goes from Normal straight to Low. The one non-multiple High is 8 slots (2013-02-09 10:00 to 13:30, between two Low blocks).
- **Normal, 6 slots (1 run):** 2013-07-25 05:00 to 07:30, between two Low blocks. The next shortest Normal stretches are 12 slots: 2013-01-28 23:00 to 2013-01-29 04:30 and 2013-08-18 02:00 to 07:30.
- **Event runs of 6 slots or fewer:** 52 of 161 (26 Low, 26 High).

## When events happen

- 161 event runs (Low + High) fall on 115 of 365 days. Events per day: 1 event on 88 days, 2 on 8 days, 3 on 19 days.
- Event start times: 17:00 (40 runs), 05:00 (39), 23:00 (34), 11:00 (18), 14:00 (7), 02:00 (6), 08:00 (6), 20:00 (6), 10:00 (3), 07:00 (2).
- 46 of 161 event runs (37 Low, 9 High) cross midnight.
- First event 2013-01-04 14:00, last event ends 2013-12-29 04:30.

## What to say

- The price level is not continuous: it changes 271 times in 2013. The median gap between events is a Normal stretch of 120 slots (60 h).
- A typical Low or High block is 12 slots (6 h). Low blocks of 24 slots (12 h) are also common (25 runs).
- Low blocks are longer on average than High blocks (18.04 vs 11.42 slots).
- 2013 only: the schedule has no rows for 2012, so any half-hour-level tariff analysis is limited to 2013.
