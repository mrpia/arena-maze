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
- The custom domain comes in a later step.

## 6. A keyboard hint on phones and tablets

**Asked (by the adult):** the same hint as arena-race, for visitors on phones or tablets who
can open the online game but can't play.

- "⌨️ You need a keyboard to play." in `KEYBOARD_HINT`, so the kids can reword it. English,
  like every other text in this game; arena-race's hint is German because its texts are.
- It shows where the main pointer is a finger (the CSS media feature `pointer: coarse`), so
  computers with a mouse or trackpad never see it. A tablet with a keyboard sees it until the
  first key press; then it's gone until the page is reloaded. Same rule as arena-race.
- This game has no start buttons to put it next to, so it's a box in the middle of the
  screen, over the game. It keeps 80 px from both sides, clear of the buttons on the left.
- Checked with touch emulation: hidden at 1280×800 with a mouse; shown at 390 px (three lines,
  x 80 to 310) and at 1024 px (one line); gone after a key press.
- Not chosen: touch controls. That's a game feature, for the kids to decide (`touch-controls`
  in the backlog).

## 7. A GitHub icon at the top

**Asked (by the adult):** the same GitHub icon as arena-race, linking to the repo.

- arena-race has it in the middle of the top line, but here the monster ability is there.
  Three places were offered: under the ability, at the bottom of the column of buttons on
  the left, or next to the ability. The adult picked under the ability: closest to
  arena-race, and like there it stays apart from the buttons that change the game.
- Same white rounded square as the buttons, with the GitHub mark inside: an inline SVG from
  `@primer/octicons` 19.15.1 (`mark-github-16.svg`, MIT), copied from arena-race. It sits at
  y 50 to 86 px, between the ability text (which ends at about 46) and the game views (which
  start at 94 in every view).
- It opens in a new tab, so a game in progress isn't lost, and lets go of focus after a click.
  The URL and the tooltip ("The game's code on GitHub") are in `GITHUB` in `config.ts`.
- Fixed in the same step: in windows 716 to 786 px wide, player 2's control line ran under
  the icon. The control lines now switch to two rows when they would come within 12 px of it
  (`MIDDLE_ROOM` in `render.ts`): below 811 px wide instead of below 716.
- Reading and downloading the code needs no account. Opening an issue or forking does, and
  GitHub accounts are for people aged 13 and up (GitHub Terms of Service, "Account Terms"),
  so the kids' issues go through an adult.
- Checked in windows 1280, 1024, 800 and 760 px wide, in all five views. A click opens
  github.com/mrpia/arena-maze in a new tab (stubbed in the test), and the page keeps the keys.
- Found while checking, not changed: the top line of the HUD overlaps in windows narrower
  than about 800 to 960 px (`narrow-hud` in the backlog).

## 8. Online at arena-maze.mrpia.ch

**Asked (by the adult):** the game at its own subdomain, as arena-race is at
arena-race.mrpia.ch.

- The adult added `arena-maze.mrpia.ch` as a custom domain of the `arena-maze` site in the
  Firebase console, and at Infomaniak, where the DNS for `mrpia.ch` is, a CNAME `arena-maze`
  → `arena-maze.web.app`: the same kind of record arena-race has.
- The certificate is managed by Google (issued by Google Trust Services, the first one valid
  from 2026-11-13 to 2027-02-11) and renews by itself. `http://` redirects to `https://`.
- It took about 30 minutes after verifying for HTTPS to work, and for a minute after that some
  requests still got Firebase's generic certificate (see `dev-notes.md`).
- https://arena-maze.web.app keeps working.
