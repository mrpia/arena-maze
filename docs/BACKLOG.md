# Backlog

Ideas and known issues not dealt with yet. Each one has a short slug, so notes, commits and
new sessions can name it ("backlog: license"). When one gets done, it becomes a step in
[decisions.md](decisions.md) and its entry here is deleted.

## license

arena-starter and arena-race are MIT licensed. This repo has no `LICENSE` file, because the
copy it came from had none (step 1). Without one, others can look at the code on GitHub but
have no permission to reuse it (GitHub Docs, "Licensing a repository").

- Adding arena-starter's MIT license would also put Team Yellow's additions under MIT, so
  Team Yellow's adult has to agree first.

## team-yellow-reasons

The game arrived as one commit (step 1), so what the kids asked for and why they chose it
isn't recorded. Team Yellow's adult could add it to step 1 of the decision log.

## game-name

"Arena Maze" for now, to match the repo (step 3). The kids may still pick a name of their own.

## touch-controls

Buttons on the screen, so the game can be played on a phone or tablet. Left open in step 6,
which added the keyboard hint instead.

- Each player has eight keys (four to move, hop, three powers): that's a lot of buttons for
  one thumb, and two players on one phone screen would be cramped.
- Touch against keyboard wouldn't be equally strong (see `CLAUDE.md`).

## hud-on-phones

Known issue, seen in step 6: in a 390 px wide window the two player names and the monster
ability in the middle of the top line are drawn on top of each other. It only matters once
`touch-controls` lets someone play on a phone.

## side-buttons-overlap

Known issue, measured in a 1280×800 window: the buttons on the left reach x = 68 px, but the
left player's view starts at x = 60 px, so they cover 8 px of it, frame included.
