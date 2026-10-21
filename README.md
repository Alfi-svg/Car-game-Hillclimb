# TESLA ROADSTER: ROCKET HILL (2D CYBER EDITION)

A complete, high-octane **2D side-scrolling hill-climb arcade racing game** featuring the Tesla Roadster concept, futuristic hill physics, rocket propulsion, 360° stunt flips, hazard avoidance, and an interactive Cyber Garage showroom.

Built with pure **HTML5 Canvas (2D Context)**, **Tailwind CSS**, and the **Web Audio API** — running completely standalone in any web browser without external server or API requirements.

---

## 🎮 PLAY NOW

### Direct Browser Access (Local Web Server)
The game is served and actively running locally on port 8000:
- **Game URL:** [http://localhost:8000/](http://localhost:8000/) or [http://localhost:8000/tesla_roadster_rocket_hill.html](http://localhost:8000/tesla_roadster_rocket_hill.html)
- You can also open [`tesla_roadster_rocket_hill.html`](file:///Users/alfi/ALfi%20game%20center/Car%20game/tesla_roadster_rocket_hill.html) or [`index.html`](file:///Users/alfi/ALfi%20game%20center/Car%20game/index.html) directly from disk in any modern web browser (Google Chrome, Safari, Microsoft Edge, Firefox).

---

## 🛠️ CRITICAL BUG FIXES & ENGINE REPAIR SUMMARY

### 1. Canvas Vector Syntax Bug Repaired
- **Diagnosis:** An erroneous expression `ctx.polygon = ctx.moveTo(46, -2);` was present in the vehicle drawing code, which corrupted the Canvas 2D context prototype and triggered silent rendering failures.
- **Fix:** Refactored into standard standard HTML5 Canvas path operations (`beginPath()`, `moveTo()`, `lineTo()`, `closePath()`, `fill()`, and `stroke()`).

### 2. Camera Spawn Offset and Viewport Freeze Fixed
- **Diagnosis:** The `Camera` instance was never reset or centered during engine initialization (`new Game()`), leaving camera coordinates at `(0, 0)` with uninitialized zoom when the menu loaded.
- **Fix:** Added explicit `this.camera.reset(this.car, this.width, this.height)` calls in the `Game` constructor and inside `handleResize()`. The camera now tracks the vehicle smoothly from the very first frame.

### 3. Startup Vehicle Drop & False Landing Impact Eliminated
- **Diagnosis:** The car was spawning at `startGroundY - 36` with zero velocity on frame 0, causing it to free-fall through the air and hit the ground with an immediate false landing thud or chassis rollover.
- **Fix:** Car initialization now queries `terrain.getHeightAt(this.x)` and positions the chassis at `y = startGroundY - this.desiredRideHeight` with the car's orientation rotated to match the initial ground slope tangent angle `Math.atan2(dy, dx)`. The car is now firmly planted on all wheels at spawn.

### 4. Crash Callback Race Condition Eliminated
- **Diagnosis:** A 1,050ms `setTimeout` was triggered in `triggerCrash()` to display the Game Over modal. If a player restarted the game quickly via Retry or Main Menu, the pending timer would fire during the new run and abruptly force the player back to the Game Over screen.
- **Fix:** Implemented an incremental run identifier (`this.runId`) and timer handle tracking (`this.crashTimeout`). When `startGame()` is called, `clearTimeout(this.crashTimeout)` cancels any pending crash timeouts, and the callback verifies `this.runId === activeRun` before showing Game Over.

### 5. Safe Game-State Reset & Persistence
- **Diagnosis:** Saved progress in `localStorage` could encounter missing keys or obsolete schema versions.
- **Fix:** `SaveManager` now validates and merges loaded JSON against full default fallback data, ensuring existing coins, high scores, upgrade levels, and newly added vehicles are preserved safely.

---

## 🚗 THE 5 PLAYABLE CYBER VEHICLES

Players can select and purchase vehicles in the interactive **Cyber Garage Showroom**. Each car features unique vector graphics, tailored dimensions, distinct wheel types, and specialized physics:

| Vehicle | Price | Class | Characteristics & Performance |
|---|---|---|---|
| **Silver Sport** | **FREE** (Starter) | Futuristic Roadster | Sleek metallic silver finish, aerodynamic glass canopy, active cold-gas rocket thruster, and sport alloy wheels. Balanced speed, acceleration, and handling. |
| **Red Racer** | **$350** | Supercar GT | High-downforce red aerodynamic chassis, twin rear ground-effect diffusers, quad yellow headlights, and high acceleration. |
| **Neon Cyber GT** | **$750** | Hypercar Concept | Deep obsidian body with cyan neon trace lines, glowing cyan cabin canopy, turbine aerodisc wheels, and high top speed with rapid rocket thrust. |
| **Offroad Beast** | **$1,200** | Cyber Truck | Heavy angular titanium exo-chassis, heavy-duty dual exposed yellow coilover springs, huge mud tires, high ride height, and extreme suspension damping. |
| **Golden Phantom** | **$2,500** | Prestige Hypercar | Reflective 24K gold mirror finish, quad amber thrusters, gold-spoked wheels, and max air control torque for effortless 360° stunt flips. |

### Live Garage Showroom Features:
- **Interactive 2D Preview Canvas:** Real-time animated vehicle preview with active suspension bounce, turning alloy wheels, and glowing headlight beams.
- **Vehicle Selection Carousel:** Easily view stats (Top Speed, Acceleration, Hill Traction, Suspension Damping).
- **Dual Tab Interface:** Toggle between the **Vehicle Showroom** (car purchasing and selection) and **Performance Upgrades**.

---

## ⚡ PERFORMANCE UPGRADE SYSTEM

Earn coins during races and spend them in the Garage across 4 performance categories (Up to Level 8):
1. **Engine Power:** Increases forward torque, allowing the vehicle to conquer steep hills and accelerate rapidly.
2. **Top Speed:** Raises maximum velocity ceiling on flats and downhills.
3. **Active Suspension:** Stiffens spring response and dampens hard landings to prevent roof rollovers.
4. **Air Control:** Boosts rotational gyro torque in mid-air, enabling quick slope alignment and 360° flips.

---

## 🏔️ PROCEDURAL HILLS & HAZARDS

- **Dynamic Terrain:** Smooth multi-frequency sine wave hills with varied slopes, valleys, and ramps.
- **Chasm Gaps:** Dangerous pits signaled by **flashing cyan warning beacons**. Players must use ramps or Rocket Boost to clear them.
- **Steep Mountain Slopes:** High incline peaks with caution chevrons that test vehicle engine torque.
- **Obstacles & Hazards:**
  - Tech Barricades & Scrap Crates.
  - Spiked Ground Mines.
  - Caution Pylons.
- **Automatic Obstacle Assistance:** Intelligent AI pilot feature that automatically fires cold-gas rocket thrusters when an impending gap or hazard collision is detected. Can be toggled ON/OFF in HUD or Settings.

---

## 🎵 WEB AUDIO SYNTHESIZER

Zero external audio files required — 100% procedurally synthesized in real time via the Web Audio API:
- **Dynamic Engine Sound:** Multi-harmonic sawtooth/triangle wave that modulates frequency and filter cutoff with vehicle velocity.
- **Rocket Thruster Roar:** Filtered white noise generator simulating high-pressure rocket exhaust.
- **Coin Chimes:** High-frequency sinusoidal chords on pickup.
- **Energy Refill:** Ascending electronic chord when collecting rocket cells.
- **Stunt Fanfare:** Triumphant arpeggio awarded upon completing 360° flips.
- **Suspension Landing Thud:** Low-frequency sine wave kick on heavy ground impacts.
- **Crash Explosion:** Resonant low-pass filtered noise burst with visual camera shake.

---

## 🕹️ CONTROLS

### Desktop Keyboard
- **Right Arrow** or **D** → Accelerate / Gas (Air: Tilt Nose Up / Backflip)
- **Left Arrow** or **A** → Brake / Reverse (Air: Tilt Nose Down / Frontflip)
- **Space** → Fire Rocket Thruster (Hold for sustained flight; double-tap for 360° stunt flip)
- **Up Arrow** / **W** → Pitch Tilt Nose Up
- **Down Arrow** / **S** → Pitch Tilt Nose Down
- **Esc** or **P** → Pause / Settings Menu

### Mobile / Touch Controls
- Dedicated on-screen touchscreen buttons for **Gas**, **Brake**, **Rocket**, and **Auto-Assist**.
- Smooth responsive layout with high-DPI canvas scaling.

---

## 📦 PROJECT DELIVERABLES

- `tesla_roadster_rocket_hill.html`: Standalone single-file production game.
- `index.html`: Entry point mirroring the main game file.
- `tesla_roadster_rocket_hill_2d.zip`: Portable distribution archive.
- `README.md`: Complete documentation and changelog.
