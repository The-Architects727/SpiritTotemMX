# SpiritTotemMX

A port of the [TOTEM](https://github.com/GEIGEIGEIST/TOTEM) split keyboard
to MX-style hot-swap switches, with a few of my own additions (per-half
battery-charge LEDs on the dongle, an on-demand battery check, and sleep
management for each half — see [Firmware](#firmware)).

This repo has everything needed to build one from scratch: the 3D-printable
case, the PCB design and fabrication files, ready-to-flash firmware, and
the full assembly guide below.

## Contents

- [What's in this repo](#whats-in-this-repo)
- [Bill of materials](#bill-of-materials)
- [Assembly](#assembly)
- [Keymap](#keymap)
- [Firmware](#firmware)
- [License](#license)

## What's in this repo

| Folder | Contents |
|---|---|
| [`Case/`](Case) | STL files for 3D printing the case and keycaps |
| [`PCB/`](PCB) | KiCad project source and fabrication-ready Gerbers/BOM/positions |
| [`Firmware/`](Firmware) | Pre-built `.uf2` firmware, ready to flash |

## Bill of materials

Links are the specific listings used for this build, not strict
requirements — quantities are usually a bulk pack, and most of these have
plenty of equivalent alternatives if a listing goes stale.

- 1 PCB — the left half, right half, and dongle are all panelized onto a
  single board you order as one piece (see [`PCB/`](PCB))
- 38× key switches — [Keychron Super Switch, tactile, 110-pack](https://www.amazon.com/dp/B0F9FJDWGZ)
- 38× Kailh hot-swap sockets — [Kailh CPG151101S11, 50-pack](https://www.amazon.com/dp/B0BVH6M5FP)
- 38× diodes (through-hole or SMD) — [1N4001, DO-41, 120-pack](https://www.amazon.com/dp/B0DN5YNFHQ)
  (1N4148 is the more common choice for keyboard matrices, but 1N4001 works
  fine here too)
- 3× Seeed Studio XIAO nRF52840 boards, with headers on —
  [Seeed Studio, 3-pack](https://www.seeedstudio.com/Seeed-Studio-XIAO-nRF52840-3PCS-p-5921.html)
- 2× JST 2.0 female wire connectors (battery leads for the two hand boards)
  and 4× JST 2.0 male PCB connectors — one kit covers both:
  [JST-PH 2.0mm 2-pin, male+female with 10cm wire, 20 sets](https://www.amazon.com/dp/B0D7Q9HJLQ)
- 2× through-hole PCB slide switches (mounted sideways so they poke out the
  side of the case) — [Chanzon SPDT 3mm slide switch, 70-pack](https://www.amazon.com/dp/B09R43HCY3)
- 2.54mm pitch headers for the XIAO standoffs — 60 pin positions total —
  [2.54mm male/female pin headers, 30-pack](https://www.amazon.com/dp/B0FCTSSLK9)
- 2× 3.7V JST 2.0 batteries, no more than 7mm thick × 20mm wide × 45mm long —
  [AKZYTUE 3.7V 750mAh LiPo, JST PH2.0](https://www.amazon.com/dp/B07TWHFW82)
- 2× 5mm LEDs
- 2× 330Ω resistors (through-hole or SMD)
- M2 brass heat-set inserts —
  [Pofsnnx M2 brass heat-set inserts, 250-pack](https://www.amazon.com/dp/B0FJXLNMLM)
- M2 screws: 6× 12mm, 1× 4mm —
  [KADRICK M2 hex socket button head screw assortment, 1000-pack](https://www.amazon.com/dp/B0DRCG3418)

## Assembly

### PCB and soldering

1. Break the panelized PCB apart into its 3 boards (left, right, dongle)
   along the mouse-bite tabs, and lightly sand the broken edges smooth.
2. Solder the diodes to the **top** face and the hot-swap sockets to the
   **underside**. The switchplate has cutouts to clear the diodes as long
   as they're soldered close to the board.
3. Solder the 2.54mm headers to the top face of each hand's board, where
   the XIAO will sit.
4. Solder the JST connectors and the slide power switch into place.
5. Solder a JST female connector pigtail to the underside of each hand's
   XIAO board, for its battery. This is the fiddliest step — put a small
   blob of solder on the pad first, then push the wire into the molten
   blob with the iron in one hand and a small pair of pliers in the other.
6. For the dongle: solder on the headers, then solder the two 5mm LEDs to
   the underside so they shine from the inside toward the outside edge,
   along with their resistors.

### Case

- `Top.stl` and `Bottom.stl` are single files — print one of each and
  mirror a second copy of each in your slicer for the other hand.
  `Switchplate.stl` and `Switchplate-left.stl` are **not** mirror images of
  each other — print each one exactly once.
- [`Case/KeyCaps/`](Case/KeyCaps) has the keycap profiles, designed to be
  printed flat on the bed while still having a swept (sculpted) profile —
  no supports needed despite the curve. Print 12× `Home`, 20×
  `Top-Bottom`, and 6× `Thumb` (38 keycaps total).
- Press a 2mm brass insert into the hole in the dongle base's dovetail.
- Press a heat-set insert into every standoff hole in each hand's bottom
  case piece.
- **Dongle:** drop the PCB+XIAO assembly into the bottom housing, slide the
  top piece on and push it forward to lock the board in place, then secure
  with a single M2×4mm screw.
- **Each hand:** place the PCB on the bottom case, then the switchplate
  over it (press a few switches into place to hold the sandwich together
  while you work), then the top plate, and secure with a 12–16mm screw in
  each top hole.

### First power-on

The dongle learns which half is "left" and which is "right" by whichever
one it bonds to first — so the very first time, power things on in this
order:

1. Everything off.
2. Plug in the dongle.
3. Power on the **left** half, and wait a moment.
4. Power on the **right** half.

After this first pairing, the two halves can be powered on in any order.

## Keymap

Colemak-DH based, 5 layers. `...` means "transparent" (falls through to
the layer below). A printable version with a visual key-by-key diagram of
every layer is at [`keymap-reference.pdf`](keymap-reference.pdf).

### Base

```
  Q      W      F      P      B        J      L      U      Y      '
  A      R      S      T      G        M      N      E      I      O
 Esc     Z      X      C      D      V        K      H      ,      .      /      -
   Ctrl/Del  Nav/Tab    Space     Shift/Ent  Sym/Bspc   Fun/\
```

### Nav

*Hold the left thumb's `Nav/Tab` key.*

```
 ...    ...    ...    ...    ...      ...    Home    Up    PgUp   End
 Gui    Alt    Ctrl   ...    ...      Del    Left   Down  Right   PgDn
 ...    ...    ...    ...    ...    ...      Ins    ...    ...    ...    ...    ...
     ...       ...       ...         ...       ...       ...
```

### Sym

*Hold the right thumb's `Sym/Bspc` key.*

```
  1      2      3      4      5        6      7      8      9      0
  !      @      #      $      %        ^      &      *      (      )
Studio   (      [      {      <      `        ~      >      }      ]      )     ...
     ...       ...       ...         ...       ...       ...
```

### Fun

*Hold the right thumb's `Fun/\` key.*

```
  F1     F2     F3     F4     F5       F6     F7     F8     F9    F10
 ...    ...    ...    ...    ...      ...    ...    ...    F11    F12
BatChk  Boot   ...    ...    ...    ...      ...    ...    ...    ...   ToGame  ...
     ...       ...       ...         ...       ...       ...
```

`BatChk` blinks the dongle's LEDs to report each half's battery charge in
10% steps. `Boot` reboots that half into its UF2 bootloader. `ToGame`
switches to the Game layer.

### Game

*Toggled from Fun's `ToGame` key; `ToBase` returns to Base.*

```
  T      Q      X      E      R        Y      U      I      O      P
  G      A      W      D      F        H      J      K      L      ;
 ...     B      Z      S      C      V        N      M      ,      .      /     ...
    Shift      Ctrl     Space       Enter      Bspc     ToBase
```

QWERTY, except W/S/X are rotated one column so A, W, and D all land on the
home row together with S directly beneath — this columnar layout puts W a
row above A/D by default, which doesn't rest as naturally for WASD as a
traditional keyboard's row stagger does. X moves up to W's old spot. The
thumb cluster is set up for typical games (Shift, Ctrl, Space on the left;
Enter, Backspace on the right) rather than mod-taps, since a key's brief
"tap" behavior is exactly the kind of thing you don't want fighting with
WASD-style movement.

## Firmware

[`Firmware/Colemak/`](Firmware/Colemak) has four ready-to-flash `.uf2`
files — one each for the left half, right half, dongle, and a
`settings_reset` image used to clear Bluetooth pairings if you ever need
to re-pair from scratch.

**To flash a board:** plug it into USB, double-tap its reset button to
drop it into bootloader mode (it shows up as a USB drive), then drag the
matching `.uf2` file onto that drive. Flash all three main boards; only
use `settings_reset` if you need to re-pair.

This firmware also includes a few extras beyond the stock TOTEM build:
per-half battery-charge LEDs on the dongle, an on-demand battery-level
check from the keyboard itself, and independent sleep for each half (with
a warning flash) that's triggered either manually or automatically when
the dongle's out of reach.

**Want to build it yourself, or modify it?** The buildable source and
GitHub Actions build workflow live in a separate repo,
[zmk-config-spirittotem](https://github.com/The-Architects727/zmk-config-spirittotem),
released under the MIT license — kept separate from this repo so the
firmware's license doesn't have to match the hardware's (see
[License](#license) below).

## License

This repo covers two different kinds of thing, deliberately under two
different licenses:

- **Case and PCB** (everything in [`Case/`](Case) and [`PCB/`](PCB)) are
  licensed under the [CERN Open Hardware Licence v2 - Permissive](LICENSE),
  the same license the original [TOTEM](https://github.com/GEIGEIGEIST/TOTEM)
  hardware design uses. This repo's hardware is a modified derivative of
  that design — see the [`LICENSE`](LICENSE) file for the specifics of
  what changed.
- **Firmware** is not included as source here (see [Firmware](#firmware))
  — the buildable source lives in
  [zmk-config-spirittotem](https://github.com/The-Architects727/zmk-config-spirittotem)
  under the MIT license. The pre-built `.uf2` binaries in
  [`Firmware/Colemak/`](Firmware/Colemak) are just compiled output from
  that source.
