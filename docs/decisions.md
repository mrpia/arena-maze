# Decision log

What was asked, what was chosen and why, oldest first.

## 1. A labyrinth with monsters (commit `15cbf25`)

Team Yellow built this game with kids on 2026-11-12, starting from
[arena-starter](https://github.com/mrpia/arena-starter). It arrived as a single commit, made
on 2026-11-13, so the steps in between and the kids' reasons aren't recorded. If you know
why something is the way it is, add it here.

What that commit has, compared with arena-starter:

- A labyrinth, new every round: 32×18 paths. After digging a maze with exactly one way
  between any two spots, each wall left standing has a 12% chance of being knocked down, so
  there are loops to run around and not only dead ends (`MAZE`).
- An arena bigger than the screen (4800×2700 world units), with cameras that follow the
  players.
- Monsters of five kinds that wander, see a slice in front of them, and chase a player they
  see. Walls block their eyes, except the Ghost's.
- Lives: 9 each, a life back every 30 s, a short safe time after a hit. A player with no
  lives is out for 5 s and comes back next to the other one. Both out at once: game over.
- Three powers per player, each on its own key with a cooldown: lightning, fire, ice.
- Every floor dresses the monsters in a costume and gives them an ability; on three floors
  they leave trails (spores, ice, fire).
- A view menu: 2D or 3D, split screen or one shared screen, and first person.
- Sound effects made by the browser, with a 🔊 button to turn them off.
- A 👹 button to pick how many monsters there are, from 0 to 30.
- A new random animal for each player every round.
- Hopping, kept from arena-starter: a hopping player can't be touched by a monster or a trail.

Not in it: arena-starter's MIT `LICENSE` file (see `license` in [BACKLOG.md](BACKLOG.md)).

## 2. A repository of its own

**Asked (by the adult):** move Team Yellow's game into its own public repo, as Team Orange's
race game went to [arena-race](https://github.com/mrpia/arena-race).

- The game wasn't on a branch of `mrpia/arena-starter`, so it came from a copy on GitHub,
  `AcaFinnofleet/game-vibe-coding`, whose owner agreed to it being copied here. Its `master`
  was pushed unchanged as `master` here, and the commit keeps its author.
- Checked before going public: the commit uses a GitHub noreply address, and there are no
  names, emails or links to people in the code or docs.
- Repo settings as for arena-race: public, issues on, wiki off.

## 3. Called "Arena Maze" (commit `f094aa6`)

**Asked (by the adult):** the same rename as arena-race got.

- "Arena 2P" became "Arena Maze": the page title, the name in `package.json` and
  `package-lock.json`, and the README and CLAUDE.md headings. The kids may still choose a
  name of their own (`game-name` in the backlog).
- Nothing is kept in `localStorage`, so nothing depends on the old name.

## 4. Docs like arena-race

**Asked (by the adult):** the same docs as arena-race.

- `CLAUDE.md` keeps Team Yellow's code rules, and gets arena-race's rules for working with
  the kids, checking a change, git and deploying.
- The README now says how the game plays: lives, powers, monsters, floors, views. One claim
  was wrong: lightning hits the nearest monster in range even behind a wall, not "the nearest
  monster in sight". `config.ts` and `powers.ts` both say so.
- `docs/` holds this log, `BACKLOG.md` and `dev-notes.md`.
- `IDEAS.md` and `IDEEN.md` removed. They were byte for byte the same as arena-starter's
  copies, which the README links to instead.
- The header comment of `maze.ts` said the labyrinth is rebuilt "whenever the arena changes
  size". The arena has a fixed size; the labyrinth is only built in `newGame()`.

## 5. Online at arena-maze.web.app

**Asked (by the adult):** build the game and put it online the same way as arena-race.

- A third Hosting site, `arena-maze`, in the Firebase project `modern-ally-102412`, next to
  math-quizz (the project's default site) and arena-race. The free Hosting quota is per
  project, so the three games share it. The game is a static bundle of 17.8 kB gzipped.
- `firebase.json` is a copy of arena-race's: it deploys to the target `game`, which
  `.firebaserc` maps to `arena-maze`, so a deploy can't land on math-quizz. Same cache
  headers: `assets/**` immutable, `/` and `/index.html` `no-cache`.
- Deploys are manual with `npm run ship`. No deploy on push.
- The custom domain comes in the next step.
