# Night Knight School — Project Guide

This guide is a handoff for a developer returning to the repository after time away. It describes the project as it exists now and separates observed facts from assumptions and recommendations. The authoritative Godot project is `knight/`.

## Purpose and current status

**Observed:** Night Knight School is intended to be a 2D side-scrolling action-adventure set around a knight school. The current playable work in `knight/` is a small platforming prototype: the knight moves and jumps through a tile-based level, collects coins, and can hit hazards. A score label and restart-on-death flow are present.

**Observed:** The repository's root `README.md` contains a game design document (GDD) describing a much broader game. Many GDD features are plans, not implemented systems; see [Planned features not implemented](#planned-features-not-implemented).

**Assessment:** Treat this as an early learning prototype, not as a complete implementation of the GDD. Prefer small, visible steps and straightforward code that is easy for a new Godot learner to understand.

## Which project to open

Open **`knight/project.godot`** and make changes in `knight/`. The repository also contains a `knights/` Godot project. The two folders were both present in the initial Git commit and began as very similar copies; subsequent development history changes `knight/`, while `knights/` remains a smaller, older snapshot. The fuller gameplay, later player animations, input settings, music setup, coin scoring, and enemy scenes are in `knight/`.

The user has confirmed that the two directories arose from poor file practices and has designated `knight/` as authoritative. Keep both directories in place for now; do not treat `knights/` as the active source or silently consolidate them.

## Opening and running

1. Open Godot Project Manager.
2. Choose **Import** and select `knight/project.godot` (or open that file directly with Godot).
3. Open the imported project and use **F5 / Run Project** to run the configured main scene. **F6 / Run Current Scene** runs whichever scene is currently open in the editor.

The configured main scene is `res://scenes/game.tscn`. The project declares Godot feature compatibility `4.7` and renderer `Forward Plus` in `knight/project.godot`. This is the declared project feature version, not a verified statement about which Godot executable is installed. The project was not launched as part of this guide, so runtime behavior and export success are not verified here.

## High-level organization

`knight/scenes/game.tscn` is the level composition: it contains the map layers, score manager and label, player instances, camera, coins, platforms, slime, and fall killzone. Reusable gameplay objects are separate scenes under `knight/scenes/`; scripts are attached to the relevant scenes and keep most behaviors local to those objects. Visual, audio, and font assets live under `knight/assets/`.

### Important scenes and scripts

| File | Responsibility |
|---|---|
| `knight/scenes/game.tscn` | Main level. Composes the tile map layers, player(s), camera, platforms, coins, slime, score display, and fall boundary. |
| `knight/scenes/player.tscn` | Character body, collision shape, knight sprite frames, and player script. Has Idle, Run, and Jump animations. |
| `knight/scenes/player2.tscn` | Second knight scene referenced by the main level. It uses the player movement script; the current settings do not give it an independent set of keyboard controls. |
| `knight/scripts/player.gd` | Applies gravity and jump, reads horizontal input, moves the character, flips the sprite, and chooses Idle/Run/Jump animation. |
| `knight/scenes/coin.tscn`, `knight/scripts/coin.gd` | Animated coin and pickup sound; on body entry, awards a point through the game manager and plays its pickup animation. |
| `knight/scripts/game_manager.gd` | Holds the session score and updates `ScoreLabel` when a point is awarded. |
| `knight/scenes/green_slime_idle.tscn`, `knight/scenes/green_slime_idle.gd` | Green slime animation, raycasts, and simple left-right patrol. Its nested killzone makes contact hazardous. |
| `knight/scenes/killzone.tscn`, `knight/scripts/killzone.gd` | Area that starts a short slow-motion death sequence, then reloads the current scene. Used for the slime and the level's fall boundary. |
| `knight/scenes/platform.tscn` | Reusable one-way platform. The main level animates one platform horizontally. |
| `knight/scenes/music.tscn` | Audio player for background music, configured to autoplay on the Music bus. |

### Autoload and project settings

`knight/project.godot` registers a `Music` autoload using `scenes/music.tscn`. This keeps music playback outside the level scene so it can persist across scene reloads. `default_bus_layout.tres` defines the Music and SFX buses. The project also has Web and macOS export presets in `knight/export_presets.cfg`.

The configured viewport is 2700 × 1000 with `canvas_items` stretch and `expand` aspect. Input actions named `P1 ...` and `P2 ...` are defined. Both sets currently map to the same keyboard keys: Space for jump and the left/right arrow keys for movement.

## Current gameplay and loop

**Observed mechanics:**

- Move horizontally and jump; gravity is applied while airborne.
- The knight faces the movement direction and switches among idle, running, and jump animations.
- Coins increment a session score, update a label, and play a pickup animation/sound.
- A green slime patrols and is dangerous on contact.
- Falling into the bottom world-boundary killzone is also dangerous.
- A hit triggers a brief half-speed sequence, then reloads the current scene. The score is held in the scene's game manager, so it resets on reload.
- A horizontally animated platform offers another basic traversal obstacle.

The current main loop is: traverse the level, jump between surfaces, collect coins, avoid the slime and falling, and restart after a hit. There is no implemented save/progression loop.

## Known inconsistencies, technical debt, and risks

These are observations from the checked-in files; behavior should be confirmed in the editor before making larger changes.

- **Two project directories:** `knight/` is authoritative, but `knights/` remains and can be opened accidentally. Its project settings and gameplay scenes differ from the active project.
- **Two player instances and unclear input intent:** `game.tscn` instantiates both `Player` and `Player2`. The current `player.gd` reads only `P2` actions, and both P1 and P2 actions map to the same keys. The actual intended single-player/two-player behavior is not established.
- **Possible collision filtering mismatch:** the main `Player` has collision layer 2, and the coin and killzone areas use collision mask 2. Check `Player2`'s effective collision layer and input behavior in Godot before relying on it as a second playable character.
- **Frame-rate-sensitive stopping:** the no-input branch in `player.gd` uses `move_toward(velocity.x, 0, SPEED)` without scaling the change by `delta`. This can make deceleration depend on the physics tick rate.
- **Assumed node structure in death handling:** `killzone.gd` directly looks for a child named `CollisionShape2D` on whichever body enters it. That works only for bodies with that expected node.
- **Project naming:** `config/name` is `Knights`, while the GDD calls the game Night Knight School.
- **Large viewport:** 2700 × 1000 is unusually large for a pixel-art prototype. Confirm the intended window and camera experience on target devices before changing it.
- **Exports are mixed with source:** exported web/macOS output files and icon variants are stored directly in `knight/`; see [Exports and generated files](#exports-and-generated-files).
- **Unused or unverified assets:** the Orc sprite sheets and several sound effects exist in assets, but are not part of the current described gameplay scenes. Their future use is unknown.
- **No runtime validation in this handoff:** this guide is based on project files and Git history; the game was not run or exported.

## Planned features not implemented

The GDD in the root `README.md` describes ideas that are not present in the current gameplay code. They should not be assumed to exist:

- Day and night phases, class attendance, studying, and school progression.
- Weapon combat and switching among sword, axe, spear, and bow; the current knight has no attack implementation.
- Health, dawn/curfew countdown, checkpoints, failure penalties, tuition credits, GPA, shops, crafting, and skill trees.
- NPC dialogue, chests, classroom doors, and interaction controls.
- Variable jump height, acceleration-based movement, and several planned hazards such as spikes, crumbling ledges, and swinging chandeliers.
- Boss encounters and the story/act progression described in the GDD.
- Gamepad controls and the GDD's A/D movement scheme. The current custom keyboard actions use Space and arrow keys.

## Recommended development direction and immediate next steps

**Recommendation:** Keep the working scope small. A polished, understandable platforming loop is a better foundation for this learning project than starting several large RPG systems at once.

1. Decide whether the next playable target is single-player or local two-player, then clarify the intended player instances and controls.
2. Make one short level loop feel complete: movement and jumping, clear coin feedback, and a predictable hazard/restart experience.
3. Choose one small feature from the GDD (for example, a basic attack or a simple checkpoint) and implement it as a separately testable scene/script behavior before adding broader progression systems.

**Structure recommendation for later:** The user has asked to leave the two-folder layout unchanged for now. When cleanup is desired, make it clear which Godot project to open and keep exported builds out of the source folders or in a Git-ignored build-output directory. Do not consolidate folders without an explicit decision.

## Exports and generated files

The Web and macOS presets in `knight/export_presets.cfg` target `./nighKnightSchool2.html` and `./nighKnightSchool2.dmg`. Multiple web-export artifacts (`.wasm`, `.pck`, `.js`, service worker, manifest, and related icon files) are checked into `knight/` alongside the source. A second set of web output uses the `nighKnightSchool` name. Git history shows exported files were added before later gameplay edits, so they may not match the current source; their exact freshness has not been confirmed.

Godot's `.godot/` directories are editor/cache output and are ignored by Git. The `.import` sidecar files beside assets are tracked import metadata. Do not confuse these sidecars with the large exported application builds.

## Confidence and open questions

- The authoritative directory is not an inference: the project owner explicitly designated `knight/`.
- Git history supports `knights/` being an older parallel copy, but does not document why both folders were created or which one the owner originally intended to continue.
- The intent behind the two player nodes, the expected target display resolution, and whether existing exports are still useful are not clear from the files alone.
- Godot feature compatibility is declared as `4.7`; the installed editor/runtime version and current runtime behavior have not been verified.
