[README_GDD.md](https://github.com/user-attachments/files/32142092/README_GDD.md)
# Game Design Document: Night Knight School

A blueprint for **Night Knight School**, a 2D side-scrolling action-adventure game blending daytime academy preparation with hazardous evening dungeon navigation.

---

## 1. Executive Summary

*   **Game Title:** Night Knight School
*   **Genre:** 2D Side-Scrolling Action-Adventure / Action RPG
*   **Platform:** PC / Console (Targeting Steam, Nintendo Switch)
*   **Target Audience:** Fans of simple 2D platformers with fun/cozy themes.
*   **Core Hook:** Balance the strict requirements of a daytime knight academy with the deadly, monster-infested curriculum that awakens on campus grounds after curfew.

---

## 2. Gameplay Mechanics & The Core Loop

### 2.1 The Dual-Phase Core Loop
The gameplay loop feeds continuously into character power and academic progression:
```
       ┌─────────────────────────────────────────┐
       ▼                                         │
[ Day Phase ] ──────────────────────────► [ Night Phase ]
Attend classes, study layouts,            Explore side-scrolling campus,
unlock skill requirements, manage gear.   slay monsters, harvest crafting components.
```

### 2.2 Movement & Physics Mechanics
*   **Side-Scrolling Navigation:** Left/Right horizontal movement tracking physics-based acceleration and friction.
*   **Jump Mechanics:** Variable jump height based on button-press duration, paired with weight-based fall gravity.
*   **Environmental Hazards:** Spikes, crumbling classroom ledges, and swinging chandelier traps requiring precise timing.

### 2.3 Input & Control Mapping Reference

| Action | Keyboard & Mouse Layout | Gamepad Layout (Xbox/Standard) | Mechanical Behavior |
| :--- | :--- | :--- | :--- |
| **Move Left / Right** | `A` / `D` | Left Stick / D-Pad | Horizontal movement with short acceleration curve. |
| **Jump** | `Spacebar` | `A` Button | Variable jump; can jump higher by holding down the button. |
| **Primary Attack** | Left Click (`LMB`) | `X` Button | Executes a directional swing or shoot based on equipped weapon. |
| **Switch Weapon** | `Q` / `E` or Scroll Wheel | `LB` / `RB` | Cycles sequentially through Sword, Axe, Spear, and Bow. |
| **Interact / Talk** | `E` | `Y` Button | Initiates NPC dialogue, opens chests, or opens classroom doors. |

---

## 3. Weapon Systems & Combat Style

The combat focuses on hot-swapping between four distinct primary weapons, each catering to different combat ranges, enemy types, and environmental puzzles.

### 3.1 Weapon Matrix

*   **The Sword (Balanced / Versatile)**
    *   *Attack Speed:* Fast
    *   *Range:* Short-Medium
    *   *Mechanical Use:* Rapid multi-hit combos. Ideal for clearing small, quick enemies and deflecting projectile projectiles.
*   **The Axe (Heavy / Destructive)**
    *   *Attack Speed:* Slow
    *   *Range:* Short
    *   *Mechanical Use:* High-damage overhead arcs. Breaks through armored enemy shields and shatters wooden barricades during exploration.
*   **The Spear (Precision / Keeping Distance)**
    *   *Attack Speed:* Medium
    *   *Range:* Medium-Long (Linear)
    *   *Mechanical Use:* Horizontal thrusting attacks. Safely keeps aggressive enemies back and hits targets through narrow wall gaps.
*   **The Bow (Ranged / Resource-Dependent)**
    *   *Attack Speed:* Medium (Requires Draw Time)
    *   *Range:* Screen-Wide (Arcing Trajectory)
    *   *Mechanical Use:* Precision ranged fire. Targets flying enemies and triggers distant environment switches. Uses a recharging quiver economy.

---

## 4. Game Rules & Economy

### 4.1 Win/Loss Boundaries
*   **Night Clear Condition:** Reach the designated safe checkpoint or headmaster dorm before the morning bell tolls.
*   **Failure State (Curfew Break):** If health reaches `0` or the countdown timer hits dawn while in a hostile zone, the player is caught by the Academy Prefects. 
*   **Failure Penalty:** Waking up in the morning infirmary with a currency penalty and a point deducted from academic standing, though key equipment remains intact.

### 4.2 Economic Systems
*   **Tuition Credits (Gold):** Harvested from nocturnal creatures or earned via high marks in daytime tests. Used to purchase weapon upgrades and crafting recipes.
*   **Grade Point Average (GPA):** Acts as a leveling mechanic. Higher GPAs grant access to advanced wings of the school library to unlock weapon skill trees.

---

## 5. Visual Hierarchy & Art Assets (2D Layout)

To maintain clarity on a fast-moving 2D side-scrolling plane, assets must follow a strict rendering hierarchy across multiple parallax layers.

### 5.1 Parallax Layer Stack

```
[ Layer 0: Background UI / HUD ]  --> Health bar, selected weapon slot, countdown timer.
[ Layer 1: Foreground Details ]   --> Classroom pillars, hanging banners that obscure view slightly.
[ Layer 2: Interactive Gameplay ] --> Night Knight (Player), Sword/Axe boxes, Monsters, Hazards.
[ Layer 3: Environment Midground]--> Structural school walls, chalkboards, library bookshelves.
[ Layer 4: Distant Background ]   --> Parallaxing Gothic towers, moonlight windows, moving clouds.
```

### 5.2 Art Design System
*   **Visual Silhouette Rule:** Player assets use a bright, sharp silhouette (silver/gold trim armor), while enemies utilize dark, jagged silhouettes with glowing neon highlights to ensure readability during chaotic fights.
*   **Color Space:** High-contrast palette shifting from warm, safe amber tones during the daytime phase to cool, oppressive blues and deep purples at night.

---

## 6. Narrative Beats & Structure

### 6.1 The Premise
The prestigious *Lucis Academy* trains the finest fighters in the realm. However, its curriculum forgot to mention one small detail: the school grounds are built directly on top of an ancient, shifting crypt. To pass the final exams, students must survive the night shifts.

### 6.2 Narrative Roadmap
1.  **Orientation Day (Prologue):** The player arrives, selects their starting base sword, and discovers the terrifying nature of night-time classes.
2.  **Midterm Mayhem (Act I):** The Bow and Spear are unlocked. The player uncovers a plot by a rogue teacher to open the crypt doors permanently.
3.  **Final Exams (Act II):** The ultimate test of skill. Navigating the depths of the lower academy foundations to defeat the Chancellor of Shadows.
