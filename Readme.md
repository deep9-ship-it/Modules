# DS90 // VX Better Vehicle Control (GT3 Carbon Edition)

An open-source, modular client-side vehicle physics, audio, VFX, and **GT3 Carbon** tuning suite built for **Brookhaven** on Roblox.

Powered by a self-healing microkernel (`VX_Kernel`) with live hot-reloading, adaptive shifting, mass-calibrated horsepower physics, and a full **6-tab GT3 Carbon** tuning menu.

---

## Quick Execute

Copy and execute this in your Roblox executor while in-game, then sit in any vehicle:

```lua
loadstring(game:HttpGet("https://raw.githubusercontent.com/deep9-ship-it/Modules/refs/heads/main/VX_Kernel", true))()
```

---

## Physics, Drivetrain & Audio Features

| Feature | How It Works |
| :--- | :--- |
| **GT3 Carbon Dashboard** | Middle-bottom HUD with 12-LED shift light strip, digital MPH speedo, shift-flashing gear box, boost PSI bar, RPM redline bar, throttle/brake bars, brake temp, and live status flags (`TCS`, `ABS`, `REV`, `2-STEP`, `CLUTCH`, `LAUNCH`, `SLIP`). |
| **Auto & Custom Horsepower** | Auto-adapts HP to your `topSpeed` via cubic aero-drag scaling—or toggle **Custom Horsepower** to dial anywhere from `100` to `2500 HP`. |
| **Mass-Calibrated Drive** | Measures real vehicle assembly mass and wheel radius (`HP/RPM` torque taper + quadratic air resistance) for authentic initial surge and second-half-of-gear resistance. |
| **Adaptive TCU Shifting** | Wheel-coupled gear-stage RPM, torque-converter launch, **Struggle/Hill Downshift** with **Anti-Hunt Hysteresis**, **UPOD Multi-Gear Kickdown**, corner lift-off hold, and braking rev-match downshifts. |
| **Redline Backfires & Flames** | Every redline upshift, rapid rev-limiter bounce, and high-RPM lift-off crackles and spits exhaust fire both locally and via server remotes. |
| **3-Stage Turbo System** | Realistic boost thresholds and spool lag (`21 / 42 / 78 PSI`) with separated **Turbo Blow-Off** (high-boost lift) and **Turbo Flutter** (partial-boost surge). |
| **10 Engine Sound Profiles** | Switch between `Default`, `McLaren F1`, `Huracan`, `Huracan 2`, `Aventador`, `Koenigsegg`, `McLaren`, `Valkyrie`, `Zonda`, and `Race` with real `refRpm` pitch curves and torque maps. |
| **Ackermann & Stability** | Low-speed inside-wheel Ackermann turn-in transitioning smoothly to high-speed steering reduction and decay. |
| **Preset Save / Load** | Save and reload your custom tuning setup (`Custom1`) or reset all settings to defaults in one click. |

---

## GT3 Carbon Tuning Menu Tabs (`VX_Panel`)

| Tab | Available Controls |
| :--- | :--- |
| **Dynamics** | `Front Grip`, `Rear Grip`, `Drivetrain` (`FWD`/`RWD`/`AWD`), `Braking Power`, `Brake Speed`, `Turbo Stage` (`1`/`2`/`3`), `Turbo Multiplier`, `Throttle Ultra`, `Weight Transfer Mode` (`Off`/`Normal`/`Drift`), `WT Intensity`, `Steering Sensitivity` |
| **Physics** | `Body Tilt Level` (`Off`–`Extreme`), `Torque Curve`, `Engine Braking & Strength`, `Engine Load & Sensitivity`, `Aero Drag`, `Aero Downforce & Coefficient`, `Weight Transfer`, `Clutch Sim & Duration`, `Cold Start & Duration`, `Idle Hunting`, `Engine Bog`, `Fuel Cut Limiter`, `TCS & Sensitivity`, `ABS`, `Brake Fade`, `Tire Temp`, `Anti-Lag`, `Telemetry HUD`, `Virtual Clutch`, `Turn Speed`, `Max Steer Angle`, `Steering Decay` |
| **Audio** | `Master Volume`, `Engine Boost`, `Pitch Offset` |
| **Engine** | 10-Profile Selector Grid, `Custom Horsepower` (`ON/OFF`), `Horsepower (100–2500 HP)`, `Turbo Spool Sound`, `Turbo Flutter Sound`, `Turbo Blow-Off Sound`, `Backfire` |
| **Controls** | `Top Speed (10–500 MPH)`, `Launch Control`, `Realistic Shifting`, `Shift Delay`, `Shifting Jolt`, `Vehicle Shaking`, `Ghost Mode`, `FOV Dynamics`, `High-Speed Steer Reduction & Factor`, `Realistic Turbo`, `Video Mode`, `Performance Mode` |
| **Lights** | `Light Pattern`, `Pattern Speed`, `Hazards in Show`, `Headlights in Show`, plus Live GT3 Status Card (`Kernel`, `State`, `Speed`, `RPM`, `Gear`, `HP`, `Profile`) |

---

## Controls & Hotkeys

| Key / Control | Action |
| :--- | :--- |
| **`GT3` Button** or **`F5`** | Open / Close Left-Side Tuning Panel |
| **`F2`** | Toggle Visual Telemetry Overlay |
| **`F3`** | Toggle Live Diagnostics Overlay |
| **`F4`** or **`V`** | Toggle Video Mode (hides HUD & game GUIs for clean recording) |
| **`C`** | Toggle Virtual Clutch (free-rev / 2-step) |
| **`F1`** | Print all module states in the `F9` console |
| **Hold `Backspace` (2s)** | Panic Shutdown (`_G.VX.shutdown()`) |

> **Note:** Driving and steering inputs use the on-screen mobile pedals/buttons via `VX_Input`.

---

## Module Architecture (14 Files)

| Module | Role |
| :--- | :--- |
| `VX_Kernel` | Microkernel, dependency sort, `VX.bus`, `VX.registry`, `VX.diagnostics`, watchdog & hot-reload |
| `VX_Core` | Default settings, physics constants, schemas, legacy migration & event definitions |
| `VX_authority` | Vehicle/chassis/wheel discovery, selective `VehicleSteering` isolation, `VX_MassTiltWeld` & restore |
| `VX_VehicleState` | Seat-driven state machine (`Disabled` → `Entered` → `TakingOver` → `Ready` ↔ `Idle` ↔ `Driving`) |
| `VX_Input` | Mobile `VehicleControls` pedal & steering input hook + virtual clutch |
| `VX_Profile` | 10 engine profiles with real sound IDs, `refRpm`, gear ratios & RPM torque curves |
| `VX_Remote` | Rate-limited server replication (`GunSounds`, `SET_BOOST`, `SET_THROTTLE_FLOAT`, lights, drift) |
| `VX_Powertrain` | Wheel-coupled RPM, torque converter launch, adaptive TCU, rev-limiter & redline backfires |
| `VX_DriveSystem` | Mass-normalized HP/RPM drive, turbo stages/lag/flutter/blow-off, Ackermann steering, TCS & ABS |
| `VX_Chassis` | Slope sensing, slip/spin detection, tire temps, aero downforce, weight transfer & engine braking |
| `VX_Audio` | Two-stage pitch smoothing (`refRpm`), PSI-gated spool, flutter/blow-off, shifts, backfires & compressor |
| `VX_Visuals` | Dynamic FOV, RPM vibration, shift/launch camera jolts, body pitch/roll & exhaust flames |
| `VX_UI` | GT3 Carbon middle-bottom dashboard, notifications, F2 Telemetry, F3 Diagnostics & F4 Video Mode |
| `VX_Panel` | GT3 Carbon left-side 6-tab tuning panel & JSON preset Save/Load/Reset |

---

## Developer Console Commands (`F9`)

| Command | Description |
| :--- | :--- |
| `_G.VX.help()` | List all kernel console commands |
| `_G.VX.getStatus()` | Inspect loaded module versions & states |
| `_G.VX.diag()` | Print live telemetry diagnostics |
| `_G.VX.getSetting(path)` | Read a setting (e.g. `"topSpeed"`, `"physics.absEnabled"`) |
| `_G.VX.setSetting(path, val)` | Update a setting live |
| `_G.VX.resetAll()` | Reset all settings back to defaults |
| `_G.VX.reload()` | Hot-reload Kernel & all modules from GitHub |
| `_G.VX.shutdown()` | Cleanly stop all modules and restore vehicle defaults |

---

## Community, Feedback & Shared Setups

Check out the YouTube Community page to leave feedback, report bugs, or share your drift & top-speed presets with others:

👉 **[DS90 YouTube Community (@ds90-t9e)](https://youtube.com/@ds90-t9e/community?si=GrhOry6Um1XuhQNk)**
