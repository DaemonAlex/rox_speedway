# Changelog

All notable changes to this project will be documented in this file.

## v2.3 — "Quality of Racing" (2026-03-02)

### Ghosting System
- **Start-of-race ghosting**: all racers are ghosted at GO to prevent first-corner pile-ups
- **Checkpoint-based unghost**: ghosting ends when ALL racers pass checkpoint 1 (configurable)
- **Timer fallback**: auto-unghost after 15 seconds if checkpoint condition isn't met
- **Lapped-player ghosting**: players a full lap behind the leader become ghosted to prevent griefing
- **Dual-speed client thread**: fast per-frame collision disable + slow 200ms alpha/entity resolution for performance
- Fully configurable via `Config.Ghosting` (can be disabled entirely)

### Race Summary UI
- **NUI results overlay** replaces notification toasts after race finishes
- Shows all drivers with position, total time, best lap, and payout
- **Podium styling**: gold/silver/bronze accents for top 3 positions
- **Best Lap badge**: star icon on the driver with the fastest single lap
- **Most Improved badge**: arrow icon on the driver who gained the most positions from grid to finish
- Auto-dismisses after 20 seconds (configurable), or close via ESC / Close button
- Falls back to legacy toast notifications when `Config.ResultsUI.enabled = false`

### Server Changes
- `SpawnRaceVehicles()` now broadcasts all race vehicle netIds to clients
- Grid order recorded at spawn for "Most Improved" calculation
- Expanded `speedway:finalRanking` payload includes name, bestLap, lapTimes, payout, gridPosition, badges
- Lapped-player detection in `speedway:updateProgress` handler

### New Net Events
- `speedway:raceVehicles` (S→C): share all race vehicle netIds
- `speedway:client:unghost` (S→C): end start-of-race ghost period
- `speedway:client:setGhosted` (S→C): toggle lapped-player ghost state

### Files Changed
- `config/config.lua` — added `Config.Ghosting` and `Config.ResultsUI`
- `server/s_main.lua` — netId broadcast, ghost timer, checkpoint unghost, lapped ghost, expanded results
- `client/c_main.lua` — ghost thread, unghost handlers, NUI results forwarding, cleanup
- `client/nui/timeout.html` — results overlay HTML/CSS/JS
- `CHANGELOG.md` — this entry

---

## 2025-11-03

Summary of changes derived from `NOTES.md`.

- HUD & Laps
  - Lap HUD now initializes to `1/x` at race start based on `speedway:prepareStart` payload (no local shadowing).
  - Server always includes `laps` in `speedway:prepareStart` payloads.

- Ranking
  - Server sorts racers by `lap > checkpoint > distance`.
  - Display inversion remains configurable via `Config.RankingInvert` (default `false`).

- Distance Metric
  - Improved monotonic progress tracking with virtual segments around Start/Finish.
  - Smoothing near checkpoints to avoid jitter.

- Debugging
  - Split debug controls:
    - `Config.DebugPrints` — console [DEBUG] logs (client/server). Default: `false`.
    - `Config.ZoneDebug` — lib.zones/polyzone visualization (red spheres). Default: `false`.
  - Deprecated: `Config.debug` (previous catch-all). Prefer the two new toggles above.

- UI/UX
  - Lobby overlay hides on prepare/start.
  - Selection countdown + auto-kick for AFK.
  - Blocking modal on kick.
  - Safer qb-input close and focus handling.

- Files impacted
  - `client/c_main.lua` — HUD lap init; distance metric; debug gating; zone debug visualization.
  - `server/s_main.lua` — Always include `laps` in start payload; debug gating.
  - `config/config.lua` — Added `DebugPrints` and `ZoneDebug`; kept `RankingInvert`.

### Validation checklist

- Restart the resource.
- Start a 3‑lap race:
  - HUD should display `Lap: 1/3` immediately at the start.
  - No console spam unless `Config.DebugPrints = true`.
  - Set `Config.ZoneDebug = true` if you want checkpoint/finish spheres for testing.
