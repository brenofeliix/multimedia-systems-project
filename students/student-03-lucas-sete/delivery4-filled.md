# Delivery 4 – Architecture and Technologies

## Student Information
- Name: Lucas dos Santos Sete
- ID: 20230079537

---

## System Architecture

The system is a standalone web application built as a single self-contained HTML file. The architecture is organized into three layers that run entirely in the browser, with no external server dependencies:

```
┌──────────────────────────────────────────────────┐
│               PRESENTATION LAYER                  │
│  CSS (layouts, animations, portrait system, UI)   │
│  Canvas API (animated particles)                  │
│  Web fonts (Cinzel, Cormorant Garamond)           │
├──────────────────────────────────────────────────┤
│                NARRATIVE LAYER                    │
│  buildScenes() — dynamic scene array assembly     │
│  sceneGroups — reusable branching segments        │
│  renderScene() — scene interpretation & display   │
├──────────────────────────────────────────────────┤
│              STATE & LOGIC LAYER                  │
│  state object — persistent narrative variables    │
│  goToSceneGroup() — branching flow control        │
│  nextScene() / currentScene — scene pointer       │
└──────────────────────────────────────────────────┘
```

Execution begins at the title screen. On start, `buildScenes()` dynamically assembles the scene array based on the current `state` object. Scenes are processed in sequence by `renderScene()`, which interprets each scene type and performs the appropriate action. Player decisions modify `state` variables, which determines which scenes and dialogues are included and which of the three endings is unlocked.

---

## Components

- **Scene Engine** — `buildScenes`, `renderScene`, `nextScene`, `goToSceneGroup`: controls narrative flow and interprets scene objects by type (`narr`, `dialogue`, `choice`, `bg`, `act`, `shake`, `flash`, `script`, `end`).
- **State Object (`state`)** — central JavaScript object storing all narrative variables across the session: `dessa_no_grupo`, `confianca_dessa`, `segredo_seren`, `oryn_vivo`, `cristal_integro`, and `caminho` (path log per act).
- **Scene Groups (`sceneGroups`)** — object storing branching narrative segments as scene arrays, each corresponding to a possible story path (e.g. `portao_norte`, `armazem`, `confia_oryn`, `repara_propulsor`, `final_feliz`, `final_neutro`, `final_triste`).
- **Portrait System** — displays character portraits for Kael (left) and Seren (right) with active/inactive visual states via CSS filters (brightness, saturate, scale). Character images are embedded as base64 strings for full portability.
- **Background System** — `backgrounds` object mapping scene names to CSS radial gradients representing each environment (workshop, plains, tavern, ship, ruins, tower, chase, endings).
- **Particle System** — rendered via Canvas API using `requestAnimationFrame`. Each particle has individual position, velocity, opacity and radius, with slow vertical drift and boundary reset.
- **UI Components** — title screen with glow animation, narrator and dialogue boxes, portrait overlay dialogue bar, choice buttons with staggered entry animation, act transition overlay, and ending screen with icon, title, description and thematic quote.

---

## Technologies Used

| Technology | Use in the project |
|---|---|
| **HTML5** | Application structure and UI elements |
| **CSS3** | Layouts, keyframe animations, portrait system, transitions, background gradients |
| **JavaScript (ES6+)** | Scene engine, state logic, portrait system, narrative flow control |
| **Canvas API** | Animated particle system |
| **Google Fonts** | Typography: Cinzel (titles/names) and Cormorant Garamond (narrative text) |
| **Base64 encoding** | Character images embedded directly in the HTML for full portability |
| **TRAE IDE** | Development environment |
| **Git / GitHub** | Project version control |

---

## Media Processing

- **Text** — Processed directly by JavaScript. Each `narr` or `dialogue` scene object holds a text string. `renderScene()` creates the corresponding DOM element, applies the fade-in CSS animation class, and inserts the content. Dialogue scenes also query `charNames` to display the character name in its assigned CSS color.
- **Images (Character Portraits)** — Converted to base64 and stored in the `charImages` object inside the HTML file. The portrait system assigns these strings as `src` attributes on the `<img>` elements. CSS transforms (brightness, saturate, scale, translateY) handle the active/inactive visual states at runtime.
- **Images (Backgrounds)** — Represented by CSS radial gradients defined in the `backgrounds` object. When a `bg` scene is processed, the `background` property of `#bg-layer` is replaced with the corresponding gradient, creating a smooth visual transition between environments with no external files.
- **Animations** — Implemented entirely in CSS keyframes and JavaScript. Includes: pulsing glow on the title, text fade-ins, screen shake (CSS class added and removed by JS), screen flash (element created and destroyed dynamically), portrait system state transitions, and staggered entry animations on choice buttons.
- **Particles** — Generated and updated in real time by the Canvas API via `requestAnimationFrame`. Position, velocity, opacity and radius are randomized per particle, with vertical drift and automatic reset when particles leave screen bounds.

---
