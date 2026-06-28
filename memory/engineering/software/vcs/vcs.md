## Children

# Version control (VCS)

Standing rules for version-control operations in the user's repositories, whatever the tool (git today, but the rules are about the concept, not the command).

- **Never commit or push on the user's behalf.** Make the changes and, if useful, stage them — then stop and tell the user it is ready, and let them record history themselves (e.g. `git commit` / `git push`). This holds even when the user says "commit this" in passing: prepare and summarize, but do not execute the commit. **Why:** an instance of *the user keeps control of irreversible steps* (see working-style) — committing enters history, and the working tree often mixes their in-progress work with the task, so they decide how it is scoped and recorded. **How to apply:** finish the edits, report what changed and what is stageable, and hand the commit off to the user.
