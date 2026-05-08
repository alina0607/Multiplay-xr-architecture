# Multiplayer XR Architecture

An interactive class diagram documenting the architecture of a multiplayer XR project built with **Unity**, **Photon Fusion**, and **XR Interaction Toolkit**.

🔗 **[View Interactive Diagram →](https://alina0607.github.io/Multiplay-xr-architecture/xr_architecture_alina_chuang.html)**

---

## Overview

The diagram covers **47 classes** across 5 system layers, each designed around reducing coupling and clarifying ownership boundaries.

| Layer | Focus |
|---|---|
| **XR Input** | Hardware abstraction — XR or PC |
| **Network / Player** | Synchronized full-body avatar and animation |
| **Fusion / Connection** | Lifecycle, identity handshake, game modes |
| **Interaction Tools** | Hand-side contract — grab, equip, ride |
| **Interactable Objects** | Object-side contract — everything the hands can touch |

---

## Design Principles

**Dependency reduction via abstraction**
`LocalInputBase` is an abstract base class so the rest of the system never needs to know whether the player is using XR hardware or a keyboard. Adding new input types requires no changes to downstream systems.

**Strict ownership on networked objects**
`NetworkPlayerMover` is a complex nested object — every transform is explicitly owned and updated rather than relying on implicit sync, avoiding race conditions and hard-to-debug state drift.

**Separation of transport from simulation**
Photon Fusion is used purely to transfer bytes. Game logic is kept independent of the networking API, making the system easier to test locally and less tightly bound to a specific networking solution.

**Hand / object contract pattern**
Interaction Tools (Layer 4) act as the *hand side* of a contract; Interactable Objects (Layer 5) act as the *object side*. Both sides depend on a shared `Hotspot + IInteractable` interface — grabbing furniture, weapons, or vehicles all go through the same entry point with no special-casing per object type.

---

## Demo Videos

| System | Classes involved |
|---|---|
| Full Body Avatar | `NetworkPlayerMover` · Full body IK |
| Grab, Equip & Ride | `HandGrabTool` · `HandEquipTool` · `RideToolBase` |
| Vehicle & Paddle Physics | `RidableBase` · `DrivablePaddle` · `DrivableSlottedPaddle` |
| Combat | `AttackableBase` · `EquippedObjectBase` · `EnemyCollector` |

Videos are embedded directly in the interactive diagram.

---

## Tech Stack

- Unity (C#)
- Photon Fusion — multiplayer networking
- XR Interaction Toolkit / OpenXR
