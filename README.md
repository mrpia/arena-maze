# Arena Maze

A maze game for two players on one keyboard. You run through a labyrinth full of monsters
and fight them off together with lightning, fire and ice. A new labyrinth every round.

Play it at https://arena-maze.mrpia.ch (you need a keyboard).

It grew out of [arena-starter](https://github.com/mrpia/arena-starter), a starting point for
two-player browser games on one keyboard (TypeScript, Canvas 2D and Vite, no game framework).
Team Yellow built it from there with kids on 2026-11-12. What changed since, and why, is in
[docs/decisions.md](docs/decisions.md).

```bash
npm install
npm run dev        # opens http://localhost:5173
```

| | Move | Hop | ⚡ Lightning | 🔥 Fire | ❄️ Ice |
|---|---|---|---|---|---|
| Player 1 | W A S D | Space | Q | E | R |
| Player 2 | Arrow keys | Enter | , | . | - (`/` on US keyboards) |

The game starts right away. Each player has 9 lives (`LIVES` in `config.ts`). A monster that
touches you takes 1 to 3 of them, then you blink for 2 seconds and nothing can hurt you. Every
30 seconds you get a life back, up to 9. Hop over a monster and it can't touch you. With no
lives left you're out for 5 seconds, then you come back next to the other player with all your
lives. If both players are out at the same time, it's GAME OVER: press Space or Enter for a new
labyrinth.

Each power has its own key and must cool down before it works again; the seconds left show
next to its emoji at the top (`POWERS` in `config.ts`):

- **⚡ Lightning** stuns the nearest monster in range for 3 seconds, even behind a wall.
- **🔥 Fire** shoots a fireball the way you last walked. Walls stop it. A monster has two ❤️;
  the second hit burns it up, and it comes back far away 10 seconds later.
- **❄️ Ice** freezes every monster close to you for 4 seconds.

There are five kinds of monster (`MONSTER_KINDS`): Grumbler, Sprinter, Giant, Stalker and
Ghost. Each sees a slice of the labyrinth in front of it, drawn on the floor. Walls block its
eyes, except the Ghost's: it floats through walls and sees through them. When it sees a player it chases them,
with red eyes, until it hasn't seen them for a while. The 👹 button on the left picks how many
monsters there are (`MONSTER_COUNTS`, `MONSTER_COUNT`).

The floor picks more than the look. Each floor dresses the monsters in a costume and gives
them a special ability, shown at the top in the middle (`FLOORS`, `MONSTER_ABILITIES`):
sticky spores on the wooden floor, an ice trail on the frozen lake, a fire trail on lava,
knight jumps on the chess board, and more.

Each round both players get a new animal at random (`PLAYER_LOOKS`); the colors stay, so
you can tell them apart. The 🔊 button turns the sound effects off.

On phones and tablets a box in the middle says you need a keyboard (`KEYBOARD_HINT` in
`config.ts`); it goes away at the first key press.

The GitHub icon at the top, under the monster ability, opens this repo in a new tab: read
the code, copy it, or open an issue with a bug or an idea (`GITHUB` in `config.ts`).

Press the key left of `1` (`§` on Swiss keyboards, `` ` `` on US ones) to show which
keys are held. Use it to check that your keyboard registers both players at once:
many laptop keyboards drop keys when several are held.

Before the labyrinth and the monsters, this was arena-starter: two characters moving around
an empty arena. Its [IDEAS.md](https://github.com/mrpia/arena-starter/blob/master/IDEAS.md)
([IDEEN.md](https://github.com/mrpia/arena-starter/blob/master/IDEEN.md) in German) has
themes, twists and example prompts for two-player games.

## Views

The top button on the left opens the view menu (`VIEWS` in `config.ts`, `VIEW_START` for the
view the game starts in):

- **3D ½ / 2D ½** – tilted or straight-down camera, split screen: each player has a half that follows them.
- **3D / 2D** – the same cameras on one shared screen that follows both players and zooms out when
  they run apart. Past the farthest zoom, the player running away pulls the other one along.
- **👁️** – first person, split screen. Up/down walk forward and back, left/right turn.

The arena is bigger than the screen (`ARENA` in `config.ts`). `PIXELS_PER_UNIT` sets the zoom,
`CAMERA_FOLLOW` how the cameras follow the players, `FIRST_PERSON` how first person looks.

## Where things are

- `src/config.ts` – players, emojis, colors, speed, keys, lives, powers, monsters, camera views, floors. Start here.
- `src/player.ts` – movement, hop, lives, bumping into each other.
- `src/maze.ts` – the labyrinth: builds it, stops things at its walls, finds the way through it.
- `src/monster.ts` – the monsters: placed at random, wander through the labyrinth, chase a player their eyes see.
- `src/powers.ts` – the players' powers: lightning, fireballs, ice.
- `src/trails.ts` – spots monsters leave on some floors (spores, ice, fire) and what they do to players.
- `src/sound.ts` – sound effects, made by the browser on the spot (no sound files).
- `src/camera.ts` – the views from above (2D, 3D) on a split or shared screen, and the cameras following the players. `project()` turns world positions into screen positions.
- `src/firstperson.ts` – first person: walls, monsters and the other player seen through a player's eyes.
- `src/render.ts` – characters, monsters and the HUD.
- `src/floors/` – one file per floor, plus `index.ts` with the list of floors.
- `src/input.ts` – keyboard state: `isDown()` for holding, `wasPressed()` for single taps.
- `src/main.ts` – canvas setup, the buttons on the left and the game loop.

`npm run build` type-checks and writes the static site to `dist/`. Its files are linked as
`/assets/…`, so it has to be served from the root of a web server; `npm run preview` does
that locally. To share the game, send the link at the top.

## Putting it online

The game is a static site on Firebase Hosting, at https://arena-maze.mrpia.ch (also
https://arena-maze.web.app). It has its own Hosting site, `arena-maze`, inside the Firebase project `modern-ally-102412`, which it
shares with math-quizz and [arena-race](https://github.com/mrpia/arena-race).

```bash
npm i -g firebase-tools          # once per machine
firebase login --no-localhost    # once per machine
npm run ship                     # builds, then deploys dist/ to the arena-maze site
```

`firebase.json` deploys to the target `game`, and `.firebaserc` maps that target to the site
`arena-maze`. Keep it that way: math-quizz lives on the project's default site, and a config
without a target could end up there.

Files in `assets/` have a content hash in their name and are cached for a year. `index.html`
is sent with `no-cache`, so the browser asks again on every load and a new deploy shows up
at once. Without it, Firebase Hosting lets browsers keep `index.html` for an hour, and an old
copy can point at a bundle that's no longer there.

DNS for `mrpia.ch` is at Infomaniak: `arena-maze` is a CNAME to `arena-maze.web.app`, set up
in the Firebase console under Hosting → `arena-maze` → custom domains. The certificate is
managed by Google and renews by itself.
