# 🧟 RTS-Project

> **Browser-based post-apocalyptic survival RTS played on real-world city maps.**  
> Lead a group of survivors, fortify shelters in real urban buildings, scavenge by daylight, and endure massive nocturnal hordes.

https://github.com/user-attachments/assets/e320f325-b424-4fbc-a5b7-15130958b8d0

---

## 📖 Overview

**RTS-Project** is a real-time strategy (RTS) survival game built for web browsers. Players take command of a group of survivors navigating the collapse of civilization within detailed real-world urban layouts. 

The game combines strategic base management, tactical squad movement, and an in-depth human condition simulation. While the game's engine is designed to support diverse global cities, its current environments are geographically modeled after urban areas in Chile, featuring **Santiago Downtown** and the riverfront city of **Valdivia**.

The project is engineered with a **strictly decoupled, deterministic architecture** prepared for a persistent, infinite-loop multiplayer world:
- **Simulation Layer (`src/sim/`)**: Pure JavaScript engine running inside an isolated Web Worker at 20 Hz, completely free of DOM or rendering dependencies.
- **Rendering Engine (`src/render/`)**: Three.js WebGL isometric engine featuring smooth zoom transitions into a 2D cartographic street map, custom shaders, and GPU-driven Vertex Animation Textures (VAT) for rendering hundreds of infected units simultaneously.
- **Survivor Simulation**: Full bodily state tracking (hunger, thirst, fatigue, sleep, and exposure), localized anatomical trauma, and a spatial grid inventory system.

---

## ✨ Key Features

### 🗺️ Geographically-Based Urban Maps
- **Procedural City Reconstruction**: Converts real-world geographic road networks and building footprints into clean 3D environments with curbs, sidewalks, intersections, and crosswalks.
- **Accessible Structures**: Every playable building features calculated, clean access points connected directly to the pedestrian road graph.
- **Current Environments**:
  - **Santiago Downtown**: Dense city blocks, avenues, historic squares, bridges, and underground metro network entrances.
  - **Valdivia**: Riverfront layout with bridges, waterways, and diverse urban density.
 
    https://github.com/user-attachments/assets/ea04c564-1817-416c-b6e2-c6c395d236c4

### 👥 Detailed Survivor Anatomy & Simulation
- **The Leader**: Your primary playable character and settlement commander. Features distinct origin backgrounds, an active skill tree (Command, Survival, Management), and squad tactical auras.
- **Physical Needs**:
  - Real-time tracking of hunger, thirst, physical stamina, sleep deprivation, and ambient cold exposure.
  - Needs directly influence movement speed, scavenging efficiency, construction speed, combat accuracy, and melee power.
- **Localized Injury & Wound System**:
  - Body parts (head, torso, arms, legs) receive distinct trauma types: scratches, lacerations, bites, or bone fractures.
  - Open bleeding requires immediate bandages to prevent blood loss; bandages soil over time and risk secondary infections.
- **Autonomous AI Without Micro-Fatigue**:
  - Survivors automatically handle routine bodily preservation (eating available rations, resting, changing bandages) based on settlement priorities, freeing the player to focus on high-level tactical and operational choices.

### 🎒 Spatial Grid Inventory
- Two-dimensional spatial item grids for survivors, vehicle trunks, and shelter stockpiles.
- Items occupy variable grid cells based on physical dimensions, supported by weight limits, pocket slots, and wearable equipment (backpacks, protective gear, flashlights).

https://github.com/user-attachments/assets/2c79c11f-d057-470e-8c43-cfe3e7524f77

### 🧟 Massive Hordes & Reactive Threat Director
- **GPU-Accelerated Crowds**: Hundreds of active infected rendered on screen via instancing and Vertex Animation Textures (VAT) without CPU skeletal overhead.
- **Distinct Threat Archetypes**: Multiple infected mutations ranging from standard walkers and agile sprinters to durable brutes and underground metro nest outbreaks.
- **Environment-Driven Difficulty**: Difficulty does not scale with calendar days. Instead, enemy pressure responds dynamically to **sector danger**, **heat generation** (noise, lights, settlement size), and a **comfort detector** designed to disrupt complacent shelters.

### 🏗️ Base Construction & Fortifications
- **Shelter Infrastructure**: Adapt ordinary city buildings into fortified Headquarters with functional modules (workshops, kitchens, farms, rainwater collectors, generators, and infirmaries).
- **Perimeter Defense**: Construct barricades, wooden and reinforced walls, automated security gates, barbed wire, rooftop watchtowers, and boarding for doors and windows.
- **Tactical Field Crafting**: Produce emergency items on the go, from improvised bandages and torches to quick field barricades.

 https://github.com/user-attachments/assets/89c599d1-d734-4c63-b509-a051283f52e0

### 📜 Narrative Events & Tactical Choices
- **Timed Contextual Encounters**: Decision cards triggered during building sweeps or street encounters, with outcomes determined by survivor traits, backgrounds, and skill checks.
- **Settlement Chronicle**: Dynamic chronological record logging the milestones, tragedies, and daily survival logs of your community.

---

## 🛠️ System Architecture

```
├── scripts/             # Offline data pipeline: map downloading, geometry cleaning, and asset optimization
├── public/
│   ├── data/            # Baked city map JSON files, ground geometry, and property overrides
│   ├── models/          # 3D assets, textures, and baked VAT animation data
├── src/
│   ├── sim/             # PURE SIMULATION ENGINE (Node.js & Web Worker compatible)
│   │   ├── systems/     # Movement, combat, vision/fog-of-war, needs, injuries, horde director
│   │   ├── content/     # Data tables: items, recipes, conditions, wounds, events, infected types
│   │   ├── nav/         # Spatial grids, road graphs, pathfinding, and flowfields
│   │   └── *.test.js    # Simulation unit tests
│   ├── render/          # GRAPHICS ENGINE (Three.js)
│   │   ├── map/         # Chunked building meshes, ground layers, streets, and dynamic lighting
│   │   ├── camera.js    # Isometric RTS camera with continuous zoom to 2D tactical street map
│   │   └── vat.js       # Vertex Animation Texture instancing for massive crowds
│   ├── net/             # Decoupled network and messaging layer (Worker / Local / Server-ready)
│   ├── ui/              # Modular native DOM/CSS tactical interface and inventory windows
│   └── input/           # Input routing, keybindings, and RTS selection handling
```

---

## 🚀 Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) (v18.0.0 or later)
- [npm](https://www.npmjs.com/) (v9.0.0 or later)

### Installation

1. Clone or download the repository:
   ```bash
   git clone <repository-url>
   cd RTS-Project
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Launch the local development server:
   ```bash
   npm run dev
   ```

4. Open your browser and navigate to:
   ```
   http://localhost:5173
   ```

---

## ⌨️ Controls & Keybindings

| Action | Input |
|---|---|
| **Pan Camera** | `W` `A` `S` `D` / Arrow Keys / Middle-Click Drag / Right-Click Drag |
| **Zoom** | Mouse Wheel (Street level $\leftrightarrow$ Tactical elevation) |
| **Tactical 2D Map** | `M` (or zoom out past tactical limit) |
| **Rotate Camera** | `Q` / `E` |
| **Context Order (Move / Attack / Scavenge)** | Right-Click target or terrain |
| **Select Units** | Left-Click / Drag Selection Box |
| **Center on Leader** | `Space` / `Home` |
| **Follow Selection** | `F` |
| **Queue Orders** | Hold `Shift` + Right-Click |
| **Control Groups** | `Ctrl` + `1`–`9` (Assign) · `1`–`9` (Recall) |
| **Halt / Stop Action** | `H` |
| **Toggle Scavenge Overlay** | `V` |
| **Rotate Inventory Item** | `R` (while dragging an item) |

---

## ⚙️ Available Scripts

- `npm run dev`: Starts the Vite development server.
- `npm test`: Runs the native Node.js simulation test suite (`node --test`).
- `npm run build`: Compiles production assets into the `dist/` directory.
- `npm run preview`: Previews the production build locally.
- `npm run fetch-map [city]`: Downloads geographic map data (cached in `scripts/.cache`) and processes building footprints (`santiago` or `valdivia`).
- `npm run build-city [city]`: Generates ground geometry, sidewalks, and intersections offline from processed JSON.
- `npm run build-models`: Optimizes and bundles 3D models.
- `npm run build-icons`: Generates the UI SVG icon library.

---

## 🔧 Developer & Debug Tools

### URL Flags
- `?sim=local`: Runs the simulation loop directly on the main thread instead of a Web Worker (useful for profiling).
- `?wasd`: Enforces classical keyboard camera navigation.

### Debug Console
Press **`` ` ``** (Backquote) or **`F9`** in development to open the in-game command terminal:
- `noche` / `hora HH:MM`: Advance in-game clock time.
- `horda`: Trigger an immediate horde assault.
- `infectados <count> <type>`: Spawn specified infected units at the cursor position.
- `dar materiales <count>`: Add building supplies and resources to settlement storage.
- `speed <multiplier>`: Modify simulation tick frequency.
- `save` / `load`: Manually save or restore simulation state.
