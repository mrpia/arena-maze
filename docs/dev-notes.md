# Dev notes

How to test a change in the browser, numbers measured in earlier steps, and traps we already
fell into. The rules for working on the game are in [CLAUDE.md](../CLAUDE.md).

## Browser test recipes (Playwright MCP)

- Start a dev server of your own on another port, without opening a browser:
  `npx vite --port 5180 --strictPort --host 127.0.0.1`. The adult may have `npm run dev` on
  5173, and `npm run dev` opens a browser window on their desktop.
- Screenshots and page snapshots land in `.playwright-mcp/` inside the folder the session
  was started in. This repo's `.gitignore` covers it; delete it afterwards anyway.
- To use game modules from inside the page, import the exact URL the game loaded. After an
  edit, Vite serves changed modules with a `?t=` timestamp, so `import('/src/config.ts')` can
  return a second, fresh copy with default values:

  ```js
  const urls = performance.getEntriesByType('resource').map((e) => e.name);
  const find = (f) => urls.filter((u) => u.includes('/src/' + f)).pop();
  const camera = await import(find('camera.ts'));
  ```

- The game starts the moment the page loads, and monsters start moving at once. To look at
  something without being chased, pick 0 with the 👹 button.

## Numbers worth knowing

- Arena: 4800×2700 world units. Labyrinth: 32×18 paths of 150 units, walls 18 thick.
- In a 1280×800 window with split screen: a 94 px band at the top for the HUD, 60 px on the
  left for the buttons, and two views of 606×698 px.
- Players run at 320 units/s. Monsters chase at 120 (Ghost) to 295 (Sprinter), so a player
  can always get away. On the street floor, 🧢 Turbo makes a chasing monster 1.4× as fast for
  1 s every 4 s, but `monster.ts` caps it at the players' speed: the Grumbler, Stalker and
  Sprinter keep pace for that second without catching up. Numbers from the code, not measured.
- The built game is 49.6 kB, 17.8 kB gzipped: `index.html` and one JS file.

## Traps

- Floor painters may leave glow or transparency switched on. `drawFloor()` wraps each painter
  in its own `save()`/`restore()`; keep it that way.
- Space and Enter are the hop keys: `input.ts` swallows them, and a button that kept focus
  would be clicked again by them. The buttons on the left let go of focus after a click.
- Every `position: fixed` element with a `transform` (the column of buttons on the left) is
  a layer of its own. A `z-index` on something inside it can't lift it over another layer;
  put the `z-index` on the outer element.
- Stop a background dev server by its process id (`ss -ltnp | grep 5180`), not with
  `pkill -f vite…`: the pattern also matches the shell running `pkill`, which kills itself
  before the rest of the command runs.
- Keyboard ghosting: each player has eight keys, and many laptop keyboards drop keys when
  several are held. The key left of 1 (§ on Swiss keyboards) shows which keys the game sees.
- A session can't change its working directory to another folder. Start Claude Code in the
  repo folder.
