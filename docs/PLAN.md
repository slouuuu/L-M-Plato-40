# Team plan

**Code freeze: Saturday 14:00** (dataset CHALLENGE.md; case book says 15:00 — confirm on Discord).
Holdout release ~11:00 Saturday. Track demo 15:00–16:00.

## Split

| Person A — data & detection | Person B — operations & presentation |
|---|---|
| S02 water (`detectors/s02_water.py`) | S01 turnaround planner (`s01_turnaround.py`) |
| S03 comfort / heat pump (`s03_comfort.py`) | Guest messages (template + optional LLM) |
| S04 maintenance (`s04_maintenance.py`) | S05 surge (`s05_surge.py`) |
| Energy / battery (`energy.py`) | Logistics (`logistics.py`) |
| Numbers for `insights.md` | Viewer, `insights.md` text, video, pitch |

## Timeline

- until 18:30 — backbone (done: loader, as-of, state, policy, episodes, CLI, validator)
- 18:30–01:00 — Level 1 (S01–S03) working on quickstart
- 01:00–07:00 — Level 2 (S04, S05, energy, logistics), sleep in 3 h shifts
- 07:00–10:30 — dev year run, tune false alarms, insights, viewer
- 10:30 — tag `pre-holdout`, test from fresh clone
- ~11:00 — holdout run unchanged, commit `decisions_holdout.jsonl`
- 11:30–14:00 — README, video, submission

## Quickstart incident map (from work_orders.csv — hints, not official ground truth)

| Scenario | What to detect |
|---|---|
| S02 loud | R104 burst hose, 13 Jan (BMS WATER_NIGHT_FLOW on WM_MAIN 01:45) |
| S02 subtle | R305 toilet fill valve running, fixed 25 Jan |
| S03 | Cold snap 14–17 Jan (−13.6 °C), HP1 COP 2.8 → 1.6, lockout_high_pressure, refrigerant leak fixed 17 Jan, TECH_1 sick 15 Jan |
| S04 | AHU1 filter_dp 92 → 165 Pa, gap 17–20 Jan, 256 Pa + airflow drop, TECH_1 vacation 19–23 Jan, filters replaced 23 Jan |
| S05 | 25 arrivals 23 Jan, 20 on 26 Jan, CLEANER_1 sick 24 Jan (known 05:30) |
| S01 | 37 early check-ins, 8 waiting guests, CLEANER_1 sick 8 + 17 Jan |
| DQ | GW_F3 outage 11 Jan, R205 frozen sensor, R106 85 °C, WM_R403 exchange (WO dated 15 Jan, reset seen 18 Jan) |

Dev year: 76 work orders → use as weak labels to measure detection rate (only as known after `created_utc`).

## Rules we never break

- No future data (use `ctx.asof`), missing ≠ zero, one episode per incident.
- Approval/consent only via `make_decision` policy; `validate.py` must PASS before every push.
