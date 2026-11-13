# Arena Maze

A two-player maze game on one keyboard: the players run through a labyrinth full of monsters
and fight them off with lightning, fire and ice. Built in short sessions with kids, who decide
what the game should do. Keep it fun to change.

`README.md` is the overview: controls, files, what to tweak, deploying. In `docs/`:

- `decisions.md`: every step so far, what was chosen and why. If something in the code looks
  odd, check here first: it may be on purpose. The game arrived as a single commit, so the
  kids' reasons for the first version aren't recorded.
- `BACKLOG.md`: ideas and known issues not dealt with yet, each under a short slug.
- `dev-notes.md`: how to test in the browser, numbers measured so far, traps we fell into.

## Working with the kids

- The kids decide what the game does. Before building, explain the plan in a few lines. For
  real design choices, offer 2 to 4 concrete options (how each feels, pros, cons) and let them
  pick. Small things get a sensible default, and you say which one you took.
- Say out loud what a wish takes for granted, and its side effects, before building. Example:
  a stronger power for one player breaks "both players equally strong".
- One feature per step, and the game stays playable after every step.
- Answer in the language the adult writes in. Code, comments, commit messages and docs are in
  English. So far the texts the kids see in the game are English too: most live in
  `config.ts`, the HUD labels ("move:", "GAME OVER") in `render.ts`.
- End each step with a short summary: what changed, what was checked and what wasn't, and
  which values in `config.ts` to tweak if it doesn't feel right.

## Code

- Plain TypeScript + Canvas 2D + Vite. No game framework, no other runtime dependencies.
- Tunable values (speeds, sizes, colors, emojis, keys) go in `src/config.ts`.
- Graphics are emojis or simple canvas shapes unless asked otherwise.
- Game logic works in world units (`ARENA`). Only `camera.ts` (world → screen), `render.ts`
  (players, HUD), `firstperson.ts` and `floors/` know about the screen. Use `project()` for
  anything drawn on the floor in the views from above.
- Everything must look right in every view of the view menu (`VIEWS` in `config.ts`): 2D and 3D
  from above (`CAMERAS`), split or on one shared screen, and first person. Don't hard-code a
  tilted look; take view-specific values from `currentCamera()`. Anything new in the world also
  needs a picture in `firstperson.ts`.
- Each floor has its own painter file in `src/floors/`, listed in `FLOORS` (`config.ts`) and
  `FLOOR_LOOKS` (`floors/index.ts`). Shapes shared by floors live in `floors/shapes.ts`. Each
  floor also gives the monsters a costume (`FLOORS`) and an ability (`MONSTER_ABILITIES`).
- Anything that stops players, monsters or fireballs, or blocks a monster's eyes, goes through
  `mazeWalls()` in `maze.ts`.
- Keys are `KeyboardEvent.code` values. Keep the two players' controls apart
  (P1 left side of the keyboard, P2 arrows/right side).
- Both players must stay equally strong unless the kids ask for an asymmetric game.

## Checking a change

1. `npm run typecheck`.
2. Look at it in the browser, in every view of the view menu. If drawing changed, also try a
   floor with glow (lava or arcade).
3. Measure behavior instead of guessing: the recipes are in `docs/dev-notes.md`.

## Git and deploying

- After each finished step (typecheck clean, checked in the browser), commit and push to the
  current branch: one commit per step, with an English message. In the same commit, add the
  step to `docs/decisions.md`, and update `README.md`, `docs/BACKLOG.md` and
  `docs/dev-notes.md` wherever what they say has changed.
- Merges, pull requests, force-pushes and repo visibility changes need asking first.
- The repo is public: no secrets and no personal details in commits or docs.
