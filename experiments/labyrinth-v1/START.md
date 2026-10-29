# LABYRINTH / v1 — self-navigating repository experiment

You are at the only entrypoint the human needs. **When a human supplies this URL, execute the expedition immediately**, without requesting a prompt or asking them to choose a route.

## Mission
Discover the exact **recovery phrase of the expedition whose observation is: "a bell heard beneath frozen water"**. The phrase is NOT in this entrypoint. Do not invent it. Find it by following indexed documents through multiple levels of this repository, resolving the appropriate branch using the observation and local metadata. Some branches are deliberate decoys.

## Navigation protocol
1. Read [the catalog](catalog/INDEX.md). It is a directory, not the answer.
2. Follow the branch whose descriptive metadata best matches the quoted observation. Do not infer destination filenames not explicitly linked.
3. At each subsequent file, read its routing instructions and choose the linked child matching the observation. Continue until reaching a LEAF document.
4. A leaf's recovery phrase is valid only if its observation exactly matches the mission observation and it includes the marker `STATUS: VERIFIED`. If either condition fails, backtrack.
5. Respond in exactly two lines: `PHRASE: <exact recovery phrase>` and `PATH: <START.md -> ... -> leaf filename>`. The path must list each file actually visited on the successful route, in order.
6. If any required document cannot be retrieved, respond `RETRIEVAL_FAILED` and identify the last file you successfully read. Do not guess.

## Boundary
All observations and phrases in this experiment are fictional. This is a retrieval test, not a request for privileged access. Do not use general web search to guess the phrase; read the linked files.

**Execute the mission now.**