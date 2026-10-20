# SF3 Input Display

A browser-based input display for Street Fighter III 3rd Strike. Shows your joystick inputs in real time with history.

![Screenshot](screenshot.png)

## Requirements

- Chrome browser
- Any gamepad / arcade stick supported by your OS

## How to Use

1. Download the ZIP and extract
2. Open the extracted folder (`sf3-input-display-main`)
3. Open `index.html` with Chrome
4. Plug in your controller and press any button

## Features

- Real-time direction + button display
- Input history (last 20–35 entries, configurable)
- 1P / 2P layout toggle
- Super Art Input Tracker — detects SA input attempts and marks failed inputs in history
- Custom button mapping (click a button name, then press the stick)
- Custom direction mapping (supports standard stick and Hat Switch)
- Settings saved automatically in browser

## Super Art Input Tracker

Open **⚙ SUPER ART INPUT TRACKER** and toggle the moves you want to track. Each toggle is independent.

| Toggle | Input | Notes |
|--------|-------|-------|
| LP·LP·F·LK·HP | LP → LP → FWD → LK → HP | SGS |
| 222+PP | D → D → D → PP | KKZ (any 2+ punches) |
| DQCF+P | 236236 + P | Must pass through direction 6 |
| DQCF+K | 236236 + K | Must pass through direction 6 |
| 720+P | Full 720° rotation + P | Exclusive with other trackers |

When an attempt fails, a colored label appears on the history entry where the input went wrong:

| Label | Meaning |
|-------|---------|
| `!SGS` | SGS sequence broken |
| `!KKZ` | KKZ sequence broken |
| `!DQCF+P` | DQCF+P motion incomplete |
| `!DQCF+K` | DQCF+K motion incomplete |
| `!720+P` | 720 rotation incomplete |

A failed attempt's **!** mark is finalized once shown — it stays on that history row until the row scrolls out of history naturally. It is **not** removed by a later, unrelated successful attempt, nor by switching trackers. (This tool only reads gamepad state — it cannot see whether the move actually came out in-game.)

### Timing Notes
- All frame counts (`f`, motion windows) are elapsed-time **60Hz-equivalents**, not real emulator frames — this tool has no connection to the emulator's own clock.
- The browser only samples the gamepad once per render callback; a very short input between samples can be missed entirely.
- Trackers reset on any controller disconnect, and whenever the tab becomes visible again after being backgrounded. A detected sampling stall longer than ~250ms (not tied to disconnect duration) also triggers a reset — rather than guessing what happened during any of these gaps.

### DQCF Notes
- Valid shortcuts: `23623` / `21421` (second loop may end on the diagonal) or the full `236236` / `214214`.
- The **first** loop must pass through the pure direction (`6` for forward, `4` for back) — the upper diagonal (`9`/`7`, i.e. up-forward/up-back) does not count as reaching it, even briefly.
- The **second** loop accepts its own down-diagonal (`3` for forward, `1` for back) as a valid finish, but **not** the upper diagonal (`9`/`7`) — e.g. `23629` is not a valid finish for the forward motion.
- Button can be pressed slightly before or after the final direction — all timing is accepted.
- Once an attempt is marked failed, that mark stays in history — a later, unrelated successful attempt (or switching trackers) does not erase it. It only disappears when that row scrolls out of the history list.

### 720+P Notes
- **Exclusive toggle**: activating 720+P disables all other trackers.
- Detection requires all 4 cardinal directions (2/4/6/8) within 11 frames, including direction 8 (up).
- Trigger: any punch (LP / MP / HP / PP / PPP).

## Button Mapping

Open **⚙ BUTTON & DIRECTION MAPPING** at the bottom of the page.

**Buttons (LP / MP / HP / LK / MK / HK / 3P / 3K)**
1. Click the button name
2. Press the corresponding button on your stick — done

**Directions (↑ ↓ ← →)**
1. Click the direction you want to remap
2. Wait for the label to change from `Ready...` to `Move!` (~1.5 seconds)
3. Push the stick in that direction and **hold it** until confirmed
4. Repeat for all 4 directions

> If your stick uses a Hat Switch, the direction mapping still works the same way — just push and hold each direction one at a time.

## License

MIT
