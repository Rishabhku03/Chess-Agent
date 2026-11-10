# Chess Agent Competition

My individual project for ECE-GY 6263 Game Theory, Fall 2026. I plan to build a Python chess agent and test whether it scores more points when it adjusts its thinking time to the clock. [Competition site](https://jiahe-zhang-t.github.io/chess-tournament/index.html)

## How the competition works

Each student builds and validates one agent. On every turn, the course runner gives the agent the current position, move history, and clock information. The agent must return a legal move in UCI notation, such as `e2e4`. Each side starts with 30 seconds, gets 0.3 seconds back after an accepted move, and has at most 5 seconds per decision or the remaining clock, whichever is less. An illegal move, crash, or timeout forfeits the game. [Student Guide](https://jiahe-zhang-t.github.io/chess-tournament/materials/guide.pdf), [competition rules](https://jiahe-zhang-t.github.io/chess-tournament/materials/rules.pdf)

Each valid entry plays 32 games against four hidden instructor opponents and four games against each other valid student entry. Both stages use positions played with colors reversed. When there are at least two valid entries, staff average the two stage scores and send the top-ranked agents to a seeded playoff. Tournament performance is worth 10 of the 100 term project points. The course also expects a controlled experiment, a presentation, and a report that explains the design and analyzes the results. [Competition format](https://jiahe-zhang-t.github.io/chess-tournament/competition.html), [project description](https://jiahe-zhang-t.github.io/chess-tournament/materials/project.pdf)

## How I plan to build the agent

1. Start with a small, dependable agent. I'll read the position from the starter API, generate legal moves with `chess`, and save a fallback before searching. I'll check that the agent can finish games as both White and Black.
2. Add alpha-beta negamax to look ahead at my moves and the opponent's replies. With iterative deepening, the agent will search one depth at a time and play the best move from its last completed search when time runs short.
3. Begin with material scoring, then add my own piece-square tables so the agent can distinguish between positions with the same material. If time allows, I'll test whether looking a little farther ahead in tactical positions helps avoid bad trades and missed threats.

## How I plan to test it

I'll use Random to catch basic faults and Greedy to see how the agent handles an opponent that goes after immediate gains. Then I'll play against the starter's shallow minimax example and earlier versions of my own agent. I'll test quiet positions, tactical positions, and endgames so I can see where a change helps and where it costs points.

For the main experiment, I'll compare a fixed thinking budget with one that adjusts to the remaining clock. I'll keep the search and evaluation identical, play the same starting positions with colors reversed, and reserve a separate set of positions for the final comparison. I'll track match points, timeouts, and time remaining. I'll review the replays to distinguish losses caused by chess decisions from losses caused by the clock.



## Local setup

1. Install `uv`. [Installation instructions](https://docs.astral.sh/uv/getting-started/installation/)
2. Download Starter v1.1 and extract it to a writable folder. A short path such as `C:\ChessTournament` works well on Windows. The starter pins CPython 3.12.13 and `chess==1.11.2`. [Starter download](https://jiahe-zhang-t.github.io/chess-tournament/downloads/Chess_Agent_Competition_Starter_v1.1.zip)
3. On Windows, run `START_HERE_WINDOWS.cmd`. On macOS or Linux, run `chmod +x START_HERE_MACOS_LINUX.sh` and then `./START_HERE_MACOS_LINUX.sh`. Chess Agent Test Space opens in a local browser tab. [Student Guide](https://jiahe-zhang-t.github.io/chess-tournament/materials/guide.pdf)
4. Copy `workspace/TEMPLATE_agent.py` to `workspace/<NetID>.py` in the extracted starter. Replace the first-legal-move policy with your implementation and set a public nickname that contains no personal identifiers.

Run these commands from the extracted starter folder. Replace `YOUR_NETID.py` with the actual filename.

```powershell
uv sync --frozen
uv run python -m chess_student_app.cli validate workspace/TEMPLATE_agent.py
uv run python -m chess_student_app.cli validate workspace/YOUR_NETID.py
uv run python -m chess_student_app.cli match --agent workspace/YOUR_NETID.py --opponent random --games 20
uv run python -m chess_student_app.cli match --agent workspace/YOUR_NETID.py --opponent greedy --games 20
```

The template checks the installation. For the final file, look for `READY TO SUBMIT` in the browser or `PASSED — SUBMISSION READY` in the CLI. Review the game reports for faults and clock usage, and save PGNs for the project analysis. Random and Greedy are local practice opponents. Staff calculate official scores separately. [Student Guide](https://jiahe-zhang-t.github.io/chess-tournament/materials/guide.pdf)


