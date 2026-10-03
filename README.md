# VIRPIL Config for Dual VPC CDT-AEROMAX (Left + Right) — Star Citizen

A dual-stick VIRPIL configuration for Star Citizen using a **left VPC CDT-AEROMAX (LAERO)**
and a **right VPC CDT-AEROMAX (RAERO)**, bridged through Joystick Gremlin and vJoy.

This profile replaces my earlier **VMAX Throttle + Aeromax-R** setup: the VMAX throttle has
been swapped out for a second AEROMAX on the left hand. It is still based on
**Subliminal's Virpil Enhanced Star Citizen Bindings** (https://subliminal.gg/bindings), adapted
for two sticks and tuned to my personal preferences.

- **Star Citizen version:** 4.10.0 LIVE (profile `KJ_410_LIVE_LAERO_RAERO`)
- **Joystick Gremlin profile version:** 9

## How it works (two-layer setup)

Star Citizen never sees the physical VIRPIL devices directly. The chain is:

```
Physical LAERO + RAERO  →  Joystick Gremlin (+ modes/overlays)  →  2 vJoy devices  →  Star Citizen
```

- **Joystick Gremlin** maps each physical button/axis (per mode) to a **vJoy** virtual button.
- **Star Citizen** binds game actions to those vJoy buttons via the layout XML.

Both files are therefore required, and they must stay in sync.

| Physical device | Feeds | Seen in SC as | Role |
|---|---|---|---|
| Left VPC CDT-AEROMAX (LAERO) | vJoy 1 | `js1` | Throttle-hand / left-side controls (85 binds) |
| Right VPC CDT-AEROMAX (RAERO) | vJoy 2 | `js2` | Stick-hand / targeting & combat (93 binds) |
| Keyboard | — | `kb1` | Doors, camera, VoIP, eye-tracking (29 binds) |

> **Note:** The old **VMAX Throttle** device entry is still present in the Joystick Gremlin
> profile but has **no bindings** (it feeds nothing). It's a harmless leftover from the previous
> config and can be deleted from the profile when convenient.

## Requirements

- [Joystick Gremlin](https://whitemagic.github.io/JoystickGremlin/) (R13.3 / profile version 9)
- [vJoy](https://sourceforge.net/projects/vjoystick/) — configure **2 vJoy devices**:
  - vJoy #1 (LEFT): **128 buttons**, 8 axes
  - vJoy #2 (RIGHT): at least **81 buttons**, 8 axes
- [HidHide](https://github.com/nefarius/HidHide) — hide the physical AEROMAX sticks from
  Star Citizen so only the vJoy devices are seen (prevents double inputs).

Credits: Joystick Gremlin by WhiteMagic, HidHide by Nefarius, base bindings by Subliminal.

## Installation

1. Install vJoy and configure the two devices as above; install HidHide and hide both physical AEROMAX sticks.
2. **Re-point the devices** in the Joystick Gremlin profile — the device GUIDs in this profile are
   specific to *my* hardware. On your machine, open the profile in Joystick Gremlin and re-assign
   `LAERO`, `RAERO` and the two vJoy outputs to your own devices.
3. Load the `.xml` Joystick Gremlin profile and activate it (**Actions → Activate**).
4. In Star Citizen, back up your current control profile, then import the layout:
   `pp_rebindkeys layout_KJ_410_LIVE_LAERO_RAERO_exported.xml` in the console (`~`), or import via
   the keybinding menu. Select the profile afterwards.

## Modes (overlays)

Mode switching lives on the **left stick (LAERO)**, buttons 21 & 22 (tempo: short vs long press),
cycling between **SCM**, **NAV**, and **Aux** modes, with voice callouts. A **Modifier** overlay is
held via LAERO button 3 (and RAERO buttons 3 / 5), exposing a second layer of vJoy buttons.

## Missile / Gun mode via the flip trigger

The **right stick flip trigger** drives both weapon modes from one physical control, via a
Joystick Gremlin macro:

- **Flip up / press** → vJoy2 button 3 → `v_toggle_missile_mode`
- **On release** → pulses vJoy1 button 123 → `v_toggle_guns_mode`

So flipping the trigger arms missiles, and releasing it returns to guns — no separate bind needed.
(`v_set_missile_mode` is intentionally left unbound.)

## Targeting ministick

The right stick's analog ministick is converted to four buttons (vJoy2 37–40) for target cycling,
with a modifier layer for the multi-tap "cycle all" variants.

## My changes vs. the base bindings

### Movement & modes
- Switched **Y** and **Z** axes (Yaw ↔ Roll)
- Unbound **v_lock_rotation** from Right Shift
- Mode switching (SCM/NAV/Aux) handled on the **left stick** rather than overlays on individual buttons

### Position moves (personal layout)

| Function | Now on (former position) | Notes |
|---|---|---|
| **Brake** | Auxiliary Mode Cycle | |
| **Auxiliary Mode Cycle** | Decoy | |
| **Operator Mode Cycle Forward** | Decoy | *(confirm vs. Aux Mode Cycle — see below)* |
| **Decoy** / **Noise** | Decouple | + modifier button |
| **Decouple** | VTOL Cycle | |
| **VTOL Cycle** | Open Door Toggle | Door buttons removed from stick |

- Changed **Capacitor Reset** and **Engineering Assignment Reset** to no longer require a modifier
- **Eject** is intentionally **unbound** (left blank) to prevent accidental ejection

### Camera & view (streaming with a left numberpad)
- **Cycle Camera View** moved from **F4** to **Z**
- **Mouse Button 3** → Freelook; **Right Alt** → 3rd-person Freelook
- Removed **Numpad 1–9** load/save views; assigned **Numpad 4–9** to camera XYZ movement

### Keyboard / misc
- **–** (minus) → VoIP push-to-talk
- **=** (equals) → toggle Tobii eye tracking
- Doors on keyboard: **LShift+D** close, **RShift+D** open, **LShift+L** lock, **RShift+L** unlock
  (door lock/unlock removed from stick buttons)

---

*Setup note I haven't resolved: in the position-moves table, **Operator Mode Cycle Forward** and
**Auxiliary Mode Cycle** both read as "former Decoy position." If one sits on a modifier layer,
I should note which; otherwise it's a collision to fix.*
