# ROX_SPEEDWAY - Custom Race Lobby System

_Originally based on [KOA_ROX_SPEEDWAY by MaxSuperTech](https://github.com/MaxSuperTech/max_rox_speedway)_

Multiplayer race system with dynamic lobbies, countdown, laps & vehicle selection for FiveM!

## Features

- **QBCore Framework** compatible
- **Dual Target System Support** - Works with both `ox_target` and `qb-target`
- **Notification System** - Supports `okokNotify`, `ox_lib`, or `rtx_notify`
- **Auto-detects Fuel System** - LegacyFuel, cdn-fuel, ox_fuel, okokGasStation, lc_fuel, or qs-fuelstations
- **Checkpoint System** - Spheres & poly-zone finish line for anti-cheat and lap detection
- **Live Position HUD** - Real-time driver position ranking during races
- **Lobby System** - Create/join custom lobbies with full management
- **Track Selection** - Multiple track layouts with configurable laps
- **Vehicle Selection** - Per-player vehicle choice from configured options
- **Countdown System** - With sound and GTA-style scaleform
- **Lap Tracking** - Full lap tracking with finish ranking screen
- **Pit Crew System** - Animated NPC pit crews with refueling and repairs
- **AMIR Leaderboard Integration** - Optional Raceway Leaderboard Display support

## Available Tracks

| Track | Checkpoints | Description |
|-------|-------------|-------------|
| Short_Track | 3 | Quick inner oval - fastest laps |
| Drift_Track | 3 | Extended route with wide sweeping turns |
| Speed_Track | 4 | Outer oval for high-speed runs |
| Long_Track | 5 | Full circuit combining all sections |

All tracks include segment hints for accurate position tracking on curves.

## Installation

1. Ensure you have the required dependencies:
   - `ox_lib`
   - `qb-core`
   - `oxmysql`
   - Either `ox_target` OR `qb-target`

2. Add the resource to your server resources folder

3. Add to your `server.cfg`:
   ```
   ensure rox_speedway
   ```

4. Configure `config/config.lua` to your preferences

## Configuration

### Target System

Choose your target system in `config/config.lua`:

```lua
Config.TargetSystem = 'ox_target'  -- or 'qb-target'
```

### Race Settings

```lua
Config.RaceStartDelay = 10      -- Countdown delay in seconds
Config.ProgressTickMs = 150     -- Position update frequency (75-200 recommended)
Config.DistanceUseNoseCorners = true  -- Better position detection on curves
```

### Notification Provider

```lua
Config.NotificationProvider = "ox_lib"  -- "ox_lib", "okokNotify", or "rtx_notify"
```

### Pit Stop Timing

Customize pit crew behavior for realism vs speed:

```lua
Config.PitStopTiming = {
    crewWalkSpeed    = 2.0,     -- 1.0 = walk, 2.0 = jog, 3.0+ = run
    refuelSteps      = 15,      -- Fuel fill increments (more = smoother)
    refuelStepMs     = 200,     -- Milliseconds per step
    repairDuration   = 3000,    -- Repair animation duration
    approachTimeout  = 8000,    -- Max wait for crew approach
    returnTimeout    = 15000,   -- Max wait for crew return
}
```

### Pit Crew Idle Animations

Add variety to pit crew idle behavior:

```lua
Config.PitCrewIdleAnims = {
    "WORLD_HUMAN_STAND_IMPATIENT",
    "WORLD_HUMAN_AA_SMOKE",
    "WORLD_HUMAN_CLIPBOARD",
    "WORLD_HUMAN_DRINKING",
}
```

### AMIR Leaderboard (Optional)

If you use Glitchdetector's Raceway Leaderboard Display, this resource can drive it live:

- Repo: [AMIR Leaderboard](https://github.com/glitchdetector/amir-leaderboard)

```lua
Config.Leaderboard = {
    enabled = true,
    updateIntervalMs = 1000,   -- Push cadence (lower can cause flicker)
    toggleIntervalMs = 2000,   -- Flip between Names <-> Times
    viewMode = "toggle",       -- "toggle" or "names"
    timeMode = "total",        -- "total" race time or "lap" time
}
```

**Runtime Commands:**
- Chat: `/lb names` or `/lb toggle`
- Console: `lb names <LobbyName>` or `lb toggle <LobbyName>`

## Adding Custom Tracks

Define checkpoints in `config/config.lua`:

```lua
Config.Checkpoints = {
    My_Custom_Track = {
        vector3(x1, y1, z1),  -- CP1
        vector3(x2, y2, z2),  -- CP2
        vector3(x3, y3, z3),  -- CP3
    },
}
```

Add segment hints for curves (optional but recommended):

```lua
Config.SegmentHints = {
    My_Custom_Track = {
        [0] = {  -- Start -> CP1
            vector3(x, y, z),  -- Hint points along the curve
        },
        [1] = {  -- CP1 -> CP2
            vector3(x, y, z),
        },
    },
}
```

## Pit Crew Zones

Configure pit stop locations:

```lua
Config.PitCrewZones = {
    {
        coords = vector3(-2865.45, 8113.30, 43.74),
        heading = 180.0,
        radius = 6.0
    },
}
```

## Debug Options

```lua
Config.DebugPrints = false  -- Console debug logs
Config.ZoneDebug = false    -- Visual zone markers
Config.RankingInvert = false  -- Invert position display if reversed
```

## Commands

| Command | Description |
|---------|-------------|
| `/speedway_cleanup` | Manual cleanup of track props and zones |
| `/lb names` | Set leaderboard to names mode |
| `/lb toggle` | Set leaderboard to toggle mode |

## Changelog

### Latest Update
- **ox_target Support** - Now works with both ox_target and qb-target
- **Full Track Configurations** - All 4 tracks now have proper coordinates
- **Pit Crew Polish** - Configurable timing, random idle animations, smoother movements
- **Segment Hints** - Added for all tracks to improve position tracking on curves
- **qs-fuelstations Support** - Added compatibility for Quasar fuel system
- **Expanded Vehicle List** - 50+ vanilla GTA vehicles including:
  - Super cars (Krieger, Emerus, Thrax, Zentorno, etc.)
  - Los Santos Tuners (Calico GTF, Jester RR, ZR350, etc.)
  - Top Sports cars (Pariah, Itali GTO, Neon, etc.)
  - Muscle cars (Dominator GT, Gauntlet Hellfire, etc.)
  - Rally cars (Omnis, GB200, Tropos Rallye)

### Previous Updates
- Bug fix: Prevent lobby/race creation when active
- Block joining after race start
- Reliable fuel sync across different fuel scripts
- Track prop and zone cleanup improvements
- LegacyFuel compatibility fixes

## Dependencies

**Required:**
- [ox_lib](https://github.com/overextended/ox_lib)
- [qb-core](https://github.com/qbcore-framework/qb-core)
- [oxmysql](https://github.com/overextended/oxmysql)

**One of:**
- [ox_target](https://github.com/overextended/ox_target) OR
- [qb-target](https://github.com/qbcore-framework/qb-target)

**Optional:**
- [AMIR Leaderboard](https://github.com/glitchdetector/amir-leaderboard)

## Credits

- Original script by [MaxSuperTech](https://github.com/MaxSuperTech/max_rox_speedway)
- Modified and enhanced by DrCannabis / DaemonAlex

## Contributing

Contributions & feedback welcome! Feel free to open issues or pull requests.

## License

See original repository for license information.
