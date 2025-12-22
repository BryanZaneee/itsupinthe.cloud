# Project: "Thailand Days" Living Art Hero Page
**Status:** In Progress
**Reference Image:** `thailand_days_portrait.jpg` (Source of Truth for composition)

## 1. Project Context & Goal
The goal is to transform the static art piece "Thailand Days" into a dynamic, "living" Hero page for the portfolio website. We are deconstructing the painting into layers to animate specific elements.

**The Vision:**
The user lands on the site, and the "Thailand Days" painting serves as the background/foreground decoration.
* **The Motorcycle:** Sits at the bottom/center (matching the painting), rumbling as if the engine is idle.
* **The Woman:** (Future) Will be sipping her drink and blinking.
* **The Atmosphere:** (Future) Smoke will puff from the exhaust.
* **The Reveal:** (Future) "Doors" or curtains will open to reveal this scene.

---

## 2. Current Assets
* [x] **Reference Composition:** `thailand_days_portrait.jpg` (Use this to align elements).
* [x] **Motorcycle Asset:** `motorcycle.png` (Isolated cutout of the boy on the bike).
* [ ] **Woman Layers:** (Pending: Separate arm, blinking eyelids, body base).
* [ ] **Smoke Assets:** (Pending: Cloud/puff textures).

---

## 3. Implementation Checklist

### Phase 1: The Rumble (Immediate Task)
*Focus: Getting the motorcycle positioned and running.*

- [ ] **Asset Preparation**
    - Ensure `motorcycle.png` has a transparent background.
    - Place file in `assets/images/motorcycle.png`.

- [ ] **HTML Structure Update**
    - Locate the `#decorations` container.
    - Create a wrapper specifically for the motorcycle to handle positioning (`bottom: 0`, centered/right per reference image).
    - **Crucial:** Implement the "Double Wrapper" technique to separate positioning from vibration:
      ```html
      <div class="cutout-wrapper-moto">      <div class="moto-rumble-container"> <img src="..." class="moto-img"> </div>
      </div>
      ```

- [ ] **CSS Styling (Positioning)**
    - Align `.cutout-wrapper-moto` to match the `thailand_days_portrait.jpg` reference.
    - Note: The bike is viewed from behind, positioned lower-center.

- [ ] **CSS Animation (The Rumble)**
    - Define `@keyframes rumble-shake`:
      - Rapid 1px-2px translations on X and Y axes.
      - Duration: `0.1s`.
      - Iteration: `infinite`.
    - Apply to `.moto-rumble-container`.

### Phase 2: Future Roadmap (Pending Assets)
*Do not implement yet, but maintain code structure to support these later.*

- [ ] **The Reveal (Doors)**
    - Plan for an overlay `<div>` that slides open (scaleX or translate) upon `window.onload`.
- [ ] **The Woman (Sipping)**
    - Plan to replace the static woman image with a layered container (`.layered-anim-container`).
    - Needs CSS anchor point logic for the elbow rotation.
- [ ] **The Smoke**
    - Plan for a `.smoke-emitter` div located at the motorcycle exhaust pipe.
    - Will require opacity and translate animations rising upward.

---

## 4. Technical Notes for Agent
* **Constraint:** Do not apply the rumble animation directly to the `<img>` tag if it has a `transform: rotate()` property, as the animation will overwrite the rotation. Use the wrapper method defined in Phase 1.
* **Responsiveness:** Ensure the motorcycle scales down appropriately on mobile devices so it doesn't cover UI elements, maintaining the aspect ratio of the original painting.