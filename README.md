# ClientMovementManager

A fully client-sided character movement controller built in Roblox Luau, focused on clean architecture, responsive gameplay feel, and scalable state-driven movement handling.

This system manages:

* Movement state detection
* Sprinting
* Jump/Fall state transitions
* Camera-relative rolling
* Character tilt simulation
* Animation state integration
* Character lifecycle handling across respawns
* Automatic cleanup through Trove

The module was designed with maintainability and scalability in mind, avoiding polling loops and stale connections while keeping gameplay responsive and deterministic.

---

# Features

## State-Driven Movement System

The controller tracks and updates multiple gameplay states in real-time:

* `IsMoving`
* `IsJumping`
* `IsFalling`
* `IsSprinting`
* `WasOnFloor`

These states are internally synchronized with:

* animation playback
* sprint logic
* roll restrictions
* airborne detection
* landing transitions

---

## Camera-Relative Character Tilt

The system applies dynamic character tilt using CFrame math to improve movement responsiveness and visual feedback.

Movement direction is projected into camera-local space through:

```lua
camera.CFrame:VectorToObjectSpace(moveDir)
```

This allows:

* forward lean while moving forward
* side tilt while strafing
* smooth interpolation back to neutral

Tilt interpolation is frame-rate independent and smoothly lerped over time.

---

## Double-Tap Roll System

The movement controller includes a fully integrated dodge/roll mechanic.

### Roll Detection

* Double-tap W/A/S/D input detection
* Per-key timestamp tracking
* Configurable tap windows

### Roll Physics

The roll system uses:

* `LinearVelocity`
* `Attachment`
* camera-relative movement vectors

instead of deprecated physics objects such as `BodyVelocity`.

The resulting roll direction is transformed from local input space into world space using the current camera orientation.

---

## Promise-Based Character Lifecycle

Rather than relying on polling loops, the movement manager uses a recursive Promise lifecycle chain to automatically handle:

* character spawning
* death cleanup
* rebinding
* state resetting
* reconnection management

Lifecycle flow:

1. Wait for character
2. Bind movement systems
3. Wait for humanoid death
4. Cleanup connections and physics
5. Await next spawn
6. Repeat

This approach prevents stale connections and keeps the system modular across respawns.

---

# Cleanup Architecture

The system uses two separate Trove instances:

## `_trove`

Persistent manager lifetime cleanup.

## `_charTrove`

Character-scoped cleanup.

This separation ensures:

* old connections do not stack across respawns
* physics constraints are cleaned correctly
* animation states are reset safely
* memory usage remains predictable

---

# Technical Highlights

* Strict Luau typing
* OOP/metatable architecture
* Promise-based async flow
* Trove cleanup management
* Camera-relative vector math
* Physics-driven movement
* Heartbeat-driven state machine
* Animation abstraction support
* Configurable constants
* State querying API

---

# APIs Used

* `RunService`
* `UserInputService`
* `Humanoid`
* `HumanoidRootPart`
* `LinearVelocity`
* `Attachment`
* `CFrame`
* `AssemblyLinearVelocity`
* `Promise`
* `Trove`

---

# Demo

The demonstration place showcases https://www.roblox.com/games/105482781630126/Movement-System:

* sprint transitions
* movement tilt
* roll mechanics
* jump/fall state detection
* animation blending
* cleanup across respawns

---

# Author

Discord: `brickcolour`
Roblox: `CoroutineLib`
