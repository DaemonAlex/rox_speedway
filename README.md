# ROX_SPEEDWAY - FiveM Race System

_Originally based on [KOA_ROX_SPEEDWAY by MaxSuperTech](https://github.com/MaxSuperTech/max_rox_speedway)_

Multiplayer race lobby system for FiveM with rewards, race classes, persistent stats, pit crews, and live position tracking.

---

## Quick Start (5 Minutes)

### What You Need

Make sure these are already running on your server:

| Resource | Where to Get It |
|----------|----------------|
| **ox_lib** | [Download](https://github.com/overextended/ox_lib) |
| **qb-core** or **qbx_core** | [QB](https://github.com/qbcore-framework/qb-core) / [Qbox](https://github.com/Qbox-project/qbx_core) |
| **oxmysql** | [Download](https://github.com/overextended/oxmysql) |
| **ox_target** or **qb-target** | [ox](https://github.com/overextended/ox_target) / [qb](https://github.com/qbcore-framework/qb-target) |

### Step-by-Step Install

1. **Download** this resource and drop the `rox_speedway` folder into your server's `resources` folder.

2. **Add this line** to your `server.cfg` (after your dependencies):
   ```
   ensure rox_speedway
   ```

3. **Restart your server.** That's it — the database table creates itself automatically.

4. **In-game:** Walk up to the race NPC at Roxwood Speedway and interact to create or join a lobby.

> **Nothing else to configure?** Nope! Defaults work out of the box. Rewards are on, entry fees are off, stats track automatically. Customize later if you want.

---

## Features

### Core Racing
- **Lobby System** — Create/join lobbies, host starts the race
- **4 Track Layouts** — Short, Drift, Speed, and Long circuits
- **Live Position HUD** — Real-time ranking during races
- **Lap Tracking** — Full lap times with podium results
- **Pit Crew** — Animated NPCs that refuel and repair your car
- **75+ Vehicles** — Curated list, no duplicates
- **LED Leaderboard** — Physical in-world scoreboard at the speedway (bundled, works out of the box)

### NEW: Race Classes
Pick a vehicle class when creating a lobby. Only vehicles in that class show up during selection.

| Class | Vehicles |
|-------|----------|
| **Open Class** | All 75+ vehicles |
| **Super Cars** | Krieger, Emerus, Deveste Eight, Vagner, Thrax, Cyclone, Ignus, Zeno, Turismo Omaggio |
| **Tuners / JDM** | Calico GTF, Jester RR, Vectre, Comet S2, Euros, ZR350, Cypher, Sultan RS Classic, Futo GTX |
| **Muscle Cars** | Dominator GT, Gauntlet Hellfire, Buffalo STX |
| **Motorcycles** | Hakuchou Drag, Bati 801RR, Shinobi, Reever, Akuma, Double T, Carbon RS |
| **Vans & Trucks** | Youga Custom, Speedo Custom, Moonbeam Custom, Surfer Custom, Yosemite 1500, Sandking XL, Kamacho, Riata |
| **Rally** | Omnis, GB200, Tropos Rallye, 2023 WRC i20, WRC 2006, Yaris WRC |

### NEW: Rewards System
Players earn cash for finishing races. Enabled by default.

- **1st place:** $5,000 | **2nd:** $3,000 | **3rd:** $1,500
- **Everyone who finishes:** $500 participation reward
- **Fastest lap bonus:** $1,000
- **Vehicle prize:** Optionally award a car to 1st place (saved to their garage)

### NEW: Entry Fees & Prize Pool
Optional buy-in system. Disabled by default — flip one toggle to enable.

- Players pay to enter; pool splits to top finishers (60/30/10)
- Refunded automatically if you leave before the race starts
- Stacks with the rewards system if both are enabled

### NEW: Persistent Race Stats
Tracks every player's career automatically. Stored in your database.

- Wins, top-3 finishes, total races, lifetime earnings
- Best lap time per track (with "NEW RECORD" alerts)
- Players can check stats anytime with `/racestats`

### Compatibility
- **Framework:** QBCore or Qbox (auto-detected)
- **Target:** ox_target or qb-target (set in config)
- **Notifications:** ox_lib, okokNotify, or rtx_notify
- **Fuel:** LegacyFuel, cdn-fuel, ox_fuel, okokGasStation, lc_fuel, qs-fuelstations (auto-detected)

---

## Available Tracks

| Track | Checkpoints | Description |
|-------|-------------|-------------|
| Short_Track | 3 | Quick inner oval — fastest laps |
| Drift_Track | 3 | Extended route with wide sweeping turns |
| Speed_Track | 4 | Outer oval for high-speed runs |
| Long_Track | 5 | Full circuit combining all sections |

All tracks include segment hints for accurate position tracking through curves.

---

## Commands

| Command | Who | Description |
|---------|-----|-------------|
| `/racestats` | Players | View your personal race statistics |
| `/lobby` | Players | Toggle lobby panel interaction (backup for keybind) |
| `/speedway_cleanup` | Players | Manual cleanup of track props and zones |
| `/lb names` | Host/Admin | Set AMIR leaderboard to names mode |
| `/lb toggle` | Host/Admin | Set AMIR leaderboard to toggle mode |

Default lobby interact key: **F2** (players can rebind in FiveM Settings > Key Bindings).

---

## Configuration Reference

Everything below is in `config/config.lua`. All settings have sensible defaults — only change what you need.

### Rewards

```lua
Config.Rewards = {
    enabled = true,
    moneyType = 'cash',              -- 'cash' or 'bank'
    payouts = {
        [1] = 5000,                  -- 1st place
        [2] = 3000,                  -- 2nd place
        [3] = 1500,                  -- 3rd place
    },
    participationReward = 500,       -- Everyone who finishes
    bestLapBonus = 1000,             -- Fastest single lap in the race
    vehiclePrize = nil,              -- Set to a model name like 'krieger' to award a car to 1st
    vehiclePrizeGarage = 'pillboxgarage',
}
```

### Entry Fees

```lua
Config.EntryFee = {
    enabled = false,                 -- Set to true to turn on buy-ins
    amount = 1000,                   -- Cost per player
    moneyType = 'cash',
    poolSplit = {
        [1] = 60,                    -- 1st gets 60% of the total pool
        [2] = 30,
        [3] = 10,
    },
}
```

### Stats

```lua
Config.Stats = {
    enabled = true,
    showAfterRace = true,            -- Show stats summary notification after each race
}
```

> The `speedway_stats` database table is created automatically when the resource starts. No SQL file to run manually.

### Race Classes

```lua
Config.RaceClasses = {
    All = {
        label = "Open Class",
        description = "Any vehicle allowed",
        vehicles = nil,              -- nil = all vehicles
    },
    Super = {
        label = "Super Cars",
        description = "Top tier supercars only",
        vehicles = { "krieger", "emerus", "deveste", "vagner", "thrax", "cyclone", "ignus", "zeno", "turismo3" },
    },
    -- ... more classes: Tuner, Muscle, Bikes, Vans, Rally
}
```

Add your own classes by adding a new key with `label`, `description`, and a `vehicles` table of model names.

### Target System

```lua
Config.TargetSystem = 'ox_target'  -- or 'qb-target'
```

### Race Settings

```lua
Config.RaceStartDelay = 10          -- Countdown seconds before GO
Config.ProgressTickMs = 150         -- Position update frequency (75-200ms recommended)
Config.DistanceUseNoseCorners = true -- Better position detection on curves
```

### Notification Provider

```lua
Config.NotificationProvider = "ox_lib"  -- "ox_lib", "okokNotify", or "rtx_notify"
```

### Pit Stop Timing

```lua
Config.PitStopTiming = {
    crewWalkSpeed    = 2.0,          -- 1.0 = walk, 2.0 = jog, 3.0+ = run
    refuelSteps      = 15,           -- Fuel fill increments
    refuelStepMs     = 200,          -- Milliseconds per step
    repairDuration   = 3000,         -- Repair animation ms
    approachTimeout  = 8000,         -- Max wait for crew approach
    returnTimeout    = 15000,        -- Max wait for crew return
}
```

### LED Leaderboard (Built-in)

Physical in-world LED scoreboard at Roxwood Speedway. Based on [glitchdetector's amir-leaderboard](https://github.com/glitchdetector/amir-leaderboard), now bundled directly — no separate resource needed.

- **During races:** Shows live player names/times with position ranking
- **When idle:** Displays the top 9 all-time best lap records, toggling between names and times
- **On first boot (no stats yet):** Shows "ROXWOOD / SPEED" as a placeholder

```lua
Config.Leaderboard = {
    enabled = true,                  -- Set to false to disable the entire board
    idleDisplay = true,              -- Show best times when no race is active
    updateIntervalMs = 1000,
    toggleIntervalMs = 2000,
    viewMode = "toggle",             -- "toggle" or "names"
    timeMode = "total",              -- "total" or "lap"
}
```

### Debug

```lua
Config.DebugPrints = false           -- Console [DEBUG] logs
Config.ZoneDebug = false             -- Red sphere zone visualization
Config.RankingInvert = false         -- Flip position display if it appears reversed
```

---

## Technical Details

### Database Schema

Auto-created on resource start via `oxmysql`:

```sql
CREATE TABLE IF NOT EXISTS speedway_stats (
    citizenid VARCHAR(50) NOT NULL,
    total_races INT DEFAULT 0,
    wins INT DEFAULT 0,
    top3 INT DEFAULT 0,
    total_earnings INT DEFAULT 0,
    best_laps JSON DEFAULT '{}',
    last_race TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (citizenid)
);
```

`best_laps` stores a JSON object keyed by track name with lap times in milliseconds:
```json
{"Short_Track": 45200, "Drift_Track": 62100}
```

### File Structure

```
rox_speedway/
  fxmanifest.lua
  config/
    config.lua            -- All configuration
  client/
    c_main.lua            -- Core client: lobby UI, race HUD, vehicle selection, rewards/stats display
    c_fuel.lua            -- Fuel system detection & helpers
    c_keys.lua            -- Vehicle key granting across key scripts
    c_customs.lua         -- Vehicle cosmetics & paint
    c_function.lua        -- Shared client utilities
    c_pit.lua             -- Pit stop logic & crew animations
    nui/
      timeout.html        -- Lobby overlay & timeout modal
  server/
    s_main.lua            -- Core server: lobbies, race logic, rewards, stats, entry fees
  leaderboard/
    cl_leaderboard.lua    -- Client: DUI setup for in-world LED sign
    sv_leaderboard.lua    -- Server: display state, idle best-times loop
    speedway.html         -- HTML/JS LED display (9 player + 1 title + 3 ad slots)
    LCDMB___.TTF          -- LCD font for the sign
    ads/                  -- 16 default ad images for the sign
  stream/
    amir_speedway_led.ydr -- 3D model for the LED sign
    amir_speedway_led.ytd -- Texture dictionary
    amir_speedway_sign.ymap -- Map placement
    def_amir_speedway.ytyp -- Type definition
  locales/
    en.lua                -- English strings (add more files for other languages)
```

### How Rewards Flow (Server)

1. Race ends (all players finish) in `speedway:lapPassed`
2. Results sorted by total time
3. `GrantRewards()` — pays each player based on position, finds best lap holder, optionally inserts vehicle prize into `player_vehicles`
4. `DistributePrizePool()` — splits entry fee pool by percentage
5. `SaveRaceStats()` — upserts `speedway_stats` with `INSERT ... ON DUPLICATE KEY UPDATE`
6. Client receives `speedway:client:rewardNotify` and `speedway:client:statsNotify` events

### Adding Custom Tracks

Define checkpoints:

```lua
Config.Checkpoints = {
    My_Custom_Track = {
        vector3(x1, y1, z1),
        vector3(x2, y2, z2),
        vector3(x3, y3, z3),
    },
}
```

Add segment hints for curves (optional but recommended for accurate ranking):

```lua
Config.SegmentHints = {
    My_Custom_Track = {
        [0] = { vector3(x, y, z) },  -- Start -> CP1
        [1] = { vector3(x, y, z) },  -- CP1 -> CP2
    },
}
```

### Adding Custom Race Classes

Add a new entry to `Config.RaceClasses`:

```lua
Config.RaceClasses.Offroad = {
    label = "Off-Road",
    description = "Dirt and mud only",
    vehicles = { "kamacho", "riata", "sandking", "bf400", "manchez3" },
}
```

Vehicle model names must match entries in `Config.RaceVehicles`. If a class has vehicles not in the master list, they won't appear in the selection dialog.

### Pit Crew Zones

```lua
Config.PitCrewZones = {
    {
        coords = vector3(-2865.45, 8113.30, 43.74),
        heading = 180.0,
        radius = 6.0
    },
}
```

---

## Changelog

### v2.2 — Security Hardening & Optimization
- **Server-side input validation** — All 8 net events now validate inputs (types, ranges, whitelists)
- **Sequential checkpoint enforcement** — Players must hit checkpoints in order; skipping is blocked
- **Full lap verification** — Laps only count if all checkpoints were passed; no more teleport-to-finish exploits
- **Removed `forcedSrc` from lapPassed** — Players can no longer complete laps for other players
- **Vehicle model whitelist** — Server rejects models not in `Config.RaceVehicles` or the selected race class
- **Rate limiting** — All net events are throttled to prevent spam/flooding
- **Refund abuse prevention** — 30-second cooldown after leaving a lobby before rejoining
- **Double-start prevention** — `startRace` blocked if race already started
- **Distance clamping** — Client progress values clamped to `[0, 15000]` with NaN rejection
- **XSS hardening** — Leaderboard and lobby UI use `textContent`/DOM API instead of `innerHTML`
- **Entity wait timeout** — Vehicle spawn waits time out after 15s instead of hanging forever
- **Memory optimization** — Leaderboard model released after texture setup; pit blips cleaned up on resource stop
- **Code deduplication** — Extracted `SpawnRaceVehicles()` replacing two identical spawn blocks

### v2.1 — Built-in LED Leaderboard
- **Bundled AMIR Leaderboard** — No separate resource needed; LED sign works out of the box
- **Idle Best-Times Display** — Board shows top 9 all-time records when no race is running
- **Auto-resume** — Idle display starts on resource boot and resumes after each race

### v2.0 — Rewards, Stats, Entry Fees & Race Classes
- **Race Classes** — 7 vehicle classes (Open, Super, Tuner, Muscle, Bikes, Vans, Rally)
- **Rewards System** — Position payouts, participation reward, best lap bonus, optional vehicle prize
- **Entry Fees** — Optional buy-in with automatic prize pool distribution
- **Persistent Stats** — Database-backed career tracking with `/racestats` command
- **75+ Vehicles** — Expanded roster with motorcycles, vans, trucks, rally cars

### v1.x — Previous Updates
- ox_target + qb-target dual support
- Full track configurations for all 4 layouts
- Pit crew system with configurable timing and animations
- Segment hints for accurate curve tracking
- AMIR leaderboard integration
- Auto-detect fuel system compatibility
- Qbox (qbx_core) framework support
- ox_lib input dialogs (replaced qb-input)

---

## Dependencies

**Required:**
- [ox_lib](https://github.com/overextended/ox_lib)
- [qb-core](https://github.com/qbcore-framework/qb-core) or [qbx_core](https://github.com/Qbox-project/qbx_core)
- [oxmysql](https://github.com/overextended/oxmysql)

**One of:**
- [ox_target](https://github.com/overextended/ox_target) OR [qb-target](https://github.com/qbcore-framework/qb-target)

**Built-in (no separate install):**
- [AMIR Leaderboard](https://github.com/glitchdetector/amir-leaderboard) by glitchdetector — bundled in `leaderboard/` and `stream/`

## Credits

- Original script by [MaxSuperTech](https://github.com/MaxSuperTech/max_rox_speedway)
- LED leaderboard sign by [glitchdetector](https://github.com/glitchdetector/amir-leaderboard)
- Modified and enhanced by DrCannabis / DaemonAlex

## Contributing

Contributions and feedback welcome! Open an issue or pull request.

## License

See original repository for license information.
