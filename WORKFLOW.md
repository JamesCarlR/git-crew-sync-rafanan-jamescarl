\# Crew Sync Workflow



\## Task 1 — Push from Clone A

Added time-and-a-half overtime pay for shifts over 8 hours in `calculatePay`. Committed and pushed from Clone A.



!\[Task 1 evidence](screenshots/task1.png)



\## Task 2 — Rejected push from Clone B

Changed `calculatePay` to round pay instead of truncating. Push was rejected because the remote had commits Clone B didn't have yet.



!\[Task 2 evidence](screenshots/task2.png)



\## Task 3 — Merge reconciliation

Merged Clone A's overtime change into Clone B. Resolved the conflict so both overtime and rounding survived. Committed and pushed.



!\[Task 3 evidence](screenshots/task3.png)



\## Task 4 — Rebase reconciliation

Added a minimum-pay floor to `calculatePay`. Push was rejected again. Fetched and rebased onto the remote's latest, resolved the conflict, and pushed without needing `--force`.



!\[Task 4a evidence](screenshots/task4a.png)

!\[Task 4b evidence](screenshots/task4b.png)



\## Task 5 — Merge into main

Merged the feature/overtime-pay branch into main (fast-forward) and pushed main.



!\[Task 5 evidence](screenshots/task5.png)



\## Task 6 — Tag v1.0-synced

Tagged the final commit as `v1.0-synced` and pushed the tag.



!\[Task 6a evidence](screenshots/task6a.png)

!\[Task 6b evidence](screenshots/task6b.png)



\## Questions



\*\*1. What did the rejected push error message tell you, and why did it happen?\*\*



The error said the remote branch contained commits I did not have locally ("fetch first"). Git refuses to overwrite remote history you haven't integrated, because that could destroy someone else's work. It happened because the two clones had each committed on top of the same base without syncing first — Clone A pushed its overtime change, then Clone B tried to push its rounding change from the older base.



\*\*2. What's the actual difference between how you resolved Task 3 (merge) vs Task 4 (rebase)?\*\*



The merge in Task 3 created a merge commit with two parents, preserving both branch histories as they happened. The rebase in Task 4 replayed my local commit on top of the remote's latest state, discarding my original commit and creating a new one with a linear history. Same conflict, different resulting commit graph.



\*\*3. What one habit would have avoided both rejected pushes in this lab?\*\*



Running `git pull` (or `git fetch` + integrate) before starting work on a branch and again before pushing. Always syncing with the remote first means you never build on a stale base, so the remote can't have commits you don't.



\*\*4. Which approach — merge or rebase — would you default to on a shared team branch, and why?\*\*



Default to merge on a shared team branch. Rebase rewrites commit hashes; if teammates already pulled the branch, they have to recover from a rewritten history, which is error-prone. Rebase is fine on a personal, unpushed local branch when a clean linear history is preferred.

