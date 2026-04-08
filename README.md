# HabitFlow-Engine

- A lightweight habit analytics engine that turns raw activity logs into measurable behavioral insight: streaks, fatigue, consistency, and recovery patterns.
- It’s built to answer a simple question: “Am I actually building a habit, or just feeling like I am?”

## Quick Start

```bash
git clone https://github.com/yourusername/HabitFlow-Engine.git
cd HabitFlow-Engine
pip install numpy
jupyter notebook notebooks/main.ipynb
```


## Demo / Preview

This runs as a Jupyter notebook locally.
After running all cells, the terminal output looks like this:

```
-----HabitFlow Results-----
___________________________
| Longest Streak:  | 14    |
| Total Failures:  | 6     |
| Collapse Events: | 6     |
| Average Fatigue: | 1.23  |
___________________________
Consistency Score: 78.4
Performance Grade: B
Motivation: Strong habits forming. A little more consistency can make you unstoppable.
```

The engine reads raw timestamped habit logs, runs a simulation over every event, and outputs a scored performance summary with a contextual motivational insight.


## Problem & Purpose

Most habit trackers just count streaks. They don't model *why* habits break or how fatigue accumulates across time. This project was built to explore what a minimal behavioral scoring engine looks like one that understands the difference between a single missed day and a pattern of collapse.

Built for:
- Developers exploring personal analytics pipelines
- Anyone curious about modeling behavioral data without reaching for pandas or sklearn
- Portfolio demonstration of low-level data processing and simulation logic in Python


## Highlights

- **Custom datetime-to-numeric conversion**: Timestamps are parsed and converted to floating-point hours-since-first-event, enabling gap analysis using `np.diff` with a `prepend` offset.
- **Fatigue simulation loop**: A stateful event-by-event simulation tracks fatigue score with asymmetric decay: +1 on miss, −0.5 on completion, floored at 0. This models recovery realistically rather than resetting on any success.
- **Strict streak rule with collapse detection**: Streak resets to 0 on any missed event; recovery flags are logged per-event so collapse frequency is measurable independently of streak length.
- **Composite consistency score**: Weighted formula combining completion rate (70%) and longest streak ratio (30%) gives a more honest performance picture than either metric alone.
- **Grade + motivation system**: Five-tier grading (A–E) maps to human-readable motivational insights, making the output usable outside of a developer context.
- **No pandas, no sklearn**: All data manipulation done directly with `np.genfromtxt`, array slicing, and `np.diff`. This was a deliberate constraint to work closer to the metal.

## Features

- Parses raw CSV habit logs with user IDs, habit names, timestamps, and statuses
- Converts string timestamps to relative numeric time (hours since first event)
- Computes inter-event time gaps for temporal pattern analysis
- Runs a full event simulation storing streak, failure count, fatigue, and collapse state per row
- Calculates a weighted consistency score and letter grade
- Outputs a clean summary table with contextual motivational feedback


## Tech Stack

| Layer | Tools |
|---|---|
| Language | Python 3.11 |
| Numerics | NumPy |
| Time Handling | Python `datetime` stdlib |
| Environment | Jupyter Notebook |
| Data Format | CSV |


## Installation & Setup

**Requirements:** Python 3.8+, pip

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/HabitFlow-Engine.git
cd HabitFlow-Engine

# 2. Install dependencies
pip install numpy jupyter

# 3. Launch the notebook
jupyter notebook notebooks/main.ipynb
```

No virtual environment is strictly required, but recommended:

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install numpy jupyter
```


## Data Format

The engine reads from `data/data.csv`. Expected columns:

```
user_id, habit_name, timestamp, status
U01, reading, 2024-01-01 07:05, done
U01, reading, 2024-01-02 07:10, done
U01, reading, 2024-01-04 07:40, missed
```

| Column | Format | Values |
|---|---|---|
| `user_id` | String | e.g. `U01` |
| `habit_name` | String | e.g. `reading` |
| `timestamp` | `YYYY-MM-DD HH:MM` | ISO datetime |
| `status` | String | `done` or `missed` |

## Usage

Open `notebooks/main.ipynb` in Jupyter and run all cells (`Run All`). The notebook processes the CSV, runs the simulation, and prints the full results summary at the end.

To test with your own data, replace `data/data.csv` with any CSV matching the format above. No code changes needed.


## Project Structure

```
HabitFlow-Engine/
├── data/
│   └── data.csv          # Raw habit log input
└── notebooks/
    └── main.ipynb        # Full engine: load → process → simulate → score
```


## Contributing

Issues and pull requests are welcome. If you're extending the engine (e.g. multi-user support, matplotlib visualization, or an API layer), open an issue first to discuss the approach.


## License

MIT License: Use it, Fork it, Build on it.


## Author

Built by [Nishant Gaurav](https://github.com/nishant05gaurav)  
[LinkedIn](https://linkedin.com/in/nishant05gaurav) · [Portfolio](https://nishant05gaurav.github.io/)