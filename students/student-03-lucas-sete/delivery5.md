# Delivery 5 – Prototype and Presentation

## Student Information
- Name: Lucas dos Santos Sete
- ID: 20230079537

---

## Prototype Description

The prototype is the fully functional version of **Echoes of Fate** — an interactive visual novel delivered as a self-contained web application (`echoes_of_fate_v3.html`) requiring no installation or external dependencies.

The prototype covers the complete three-act narrative arc, from the prologue to three distinct endings, and implements all core systems: branching dialogues with persistent consequences, a portrait system for main characters in visual novel style, five narrative variables that shape character behavior and the story's outcome, and visual effects for atmosphere (particles, transitions, screen shake, flash).

**Playable characters:** Kael (Void, mechanic) and Seren (Void, herbalist).

**Supporting characters:** Dessa (deserter), Oryn (ex-archivist), Vila (smuggler), Nameless Shadow (tragic antagonist).

**Available endings:**
- **Happy Ending — Witnesses:** the Shadow is heard and dissolves as expansion; the Crystal stabilizes and the world is saved.
- **Neutral Ending — The Remaining Thread:** partial victory; some cities fall, the Shadow persists as a thread of energy.
- **Sad Ending — Echoes in the Void:** the Crystal implodes; Kael and Seren stay behind together as the world falls.

---

## Navigation Flow

The system follows a linear flow with player-controlled branches:

```
[Title Screen]
       │
       ▼
[Prologue — Kael's Workshop / Escape from the burning city]
       │
[DECISION 1: How to leave the city?]
       ├──► North Gate (disguise with Mochi the cat)
       │         └── Dessa does not join the group
       └──► Old Warehouses
                 └── finds and recruits Dessa → dessa_no_grupo = true
       │
       ▼
[Plains — Journey to Posto Cinza]
       │
       ▼
[Tavern — Meeting Oryn and Vila]
       │
[DECISION 2: What to do with Oryn?]
       ├──► Trust Oryn → oryn_vivo = true; receives full lore about the Crystal
       └──► Distrust Oryn → cristal_integro = false; Oryn follows discreetly
       │
       ▼
[Vila's Ship — Sky chase]
       │
[DECISION 3: How to survive the pursuit?]
       ├──► Kael repairs the propulsor on the external hull
       └──► Seren brews a focus potion for Dessa → confianca_dessa += 1
       │
       ▼
[Ruins of Old Aerith — Exploration and memory echoes]
       │
       ▼
[Confrontation with the Nameless Shadow]
       │
[FINAL DECISION]
       ├──► "We witness you. You may go in peace." → HAPPY ENDING
       ├──► "There is a middle ground." → NEUTRAL ENDING
       └──► "I don't trust you." → SAD ENDING
```

The player advances by clicking anywhere on the screen or pressing Space/Enter. At decision points, choice buttons replace the advance action. There is no backward navigation — each session follows a linear path within the chosen branches.

---

## Screens / Interface

- **Title Screen** — Dark radial gradient background with CSS star decorations and a translucent floating city effect. "ECHOES OF FATE" title in Cinzel font with pulsing glow animation. Italic tagline, gold separator and start button. Animated Canvas API particle system in the background.
- **Narrator Screen** — Centered `#scene-box` with semi-transparent dark background and a soft gold border. Text in Cormorant Garamond, cream color, with fade-in animation. No character identified — used for environment descriptions and narrative transitions.
- **Dialogue Screen (Secondary Characters)** — Same central box with a dialogue block: character name in its assigned color (Dessa in red, Oryn in blue, Vila in sand, Shadow in pale blue) and speech text below.
- **Dialogue Screen — Portrait System (Kael and Seren)** — Full-height portraits on each side: Kael on the left, Seren on the right. The active character receives elevated brightness (`brightness(1.05)`) and normal scale; the inactive one is darkened (`brightness(0.35), saturate(0.4)`) and slightly receded (`scale(0.97), translateY(8px)`). CSS gradients on portrait edges blend the images into the background. Speech appears in an overlay dialogue bar between the two portraits, with the character name in their color and text in a fade-in animation.
- **Choice Screen** — Decision buttons with a bold label and an italic sub-label describing the character's intent. Staggered entry animation (each button appears with progressive delay). The narrative box is cleared before choices are displayed.
- **Act Transition Screen** — Full-screen overlay with dark background, act number in gold and act title in smaller font. Fade-in/out animation. Separates the Prologue, Act 1, Act 2 and Act 3.
- **Ending Screen** — Thematic icon, ending title in Cinzel with the corresponding color (gold for happy, blue-grey for neutral, dark red for sad), narrative description of the outcome, italic quote and restart button.

---

## System Explanation

**Initialization** — On clicking "Start", the title screen fades out, `initPortraits()` loads the base64 character images into the portrait panel `<img>` elements, and `buildScenes()` assembles the main scene array. The `currentScene` pointer is set to 0 and `renderScene()` is called for the first time.

**Scene cycle** — `renderScene()` reads `scenes[currentScene]` and branches by type: `narr`/`dialogue` creates a DOM element with the text and shows the continue button; `dialogue` for Kael or Seren triggers the portrait system instead of the central box; `choice` renders decision buttons where each calls `action()`, modifying `state` and calling `goToSceneGroup()`; `bg` updates the `#bg-layer` background; `act` displays the act transition overlay; `shake` adds and removes a CSS class on the scene box; `flash` creates and destroys a white flash element; `script` runs an invisible function (e.g. `state.segredo_seren = true`) and advances automatically; `end` renders the corresponding ending screen.

**Portrait System** — When `renderScene()` detects a Kael or Seren dialogue, it calls `showPortraitDialogue(char, text)`. The function determines the side (left/right), applies `active`/`inactive` CSS classes to both portrait panels, populates the overlay dialogue bar, and adds the `portrait-mode` class to `<body>`, which repositions the continue button above the bar. When the turn passes to a character without a portrait or to the narrator, `hidePortraits()` removes all classes and hides the panels.

**Dynamic scene assembly** — `buildScenes()` calls scene functions that use the spread operator (`...planicie_com_dessa()`, `...navio_scene()`, etc.) to build the array. Conditional dialogues use `state.dessa_no_grupo`, `state.oryn_vivo` and similar flags with short-circuit evaluation to include or omit lines at assembly time.

**Branching scene groups** — `goToSceneGroup(name)` replaces the `scenes` array with the content of the corresponding group, resets `currentScene` to 0 and calls `renderScene()`. This allows each decision to load an entirely different narrative segment without code duplication.

---

## Final Considerations

**What was learned:**

The main lesson was that branching narratives require treating story state as first-class data from the start. Every variable in `state` must be mapped against all the points where it is read before being written — otherwise inconsistencies arise where characters act without memory of the player's earlier choices, breaking narrative coherence.

The portrait system showed that visual differentiation between active and inactive characters — using only CSS brightness and scale — has significant impact on perceived quality, bringing the experience closer to commercial visual novel productions without high implementation cost.

Using a single self-contained HTML file with base64-embedded assets proved ideal for prototyping and distribution: the file runs in any modern browser without installation, server configuration or external file dependencies.

**What could be improved:**

- Integrate audio — atmospheric BGM and SFX — planned in the delivery 2 specifications but not yet present in the prototype.
- Implement video cutscenes at high-impact narrative moments (the naval chase, the Crystal's collapse).
- Add the point-and-click system for exploring lore details in the backgrounds, also specified in delivery 2.
- Implement a dialogue backlog so the player can review previous lines.
- Add a save system for multiple sessions and exploration of different paths without restarting from the beginning.
- Replace the current CSS gradient backgrounds with illustrated artwork to reach the visual level planned for the final product.

---
