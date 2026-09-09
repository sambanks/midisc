# midisc (`1.40MSC`)

MIDI scene locks for Elektron Octatrack **MK1/MK2** on stock OS **1.40C**.

Scene hold edits, XF morph (VOICE), durable part save/reload, scene clear/copy/paste, scene+encoder unlock.

## Requirements

- Python 3.10+
- Stock firmware file (you supply; not redistributed here):

  `downloads/extracted/OCTATRACK_OS1.40C.syx`

## Build

```bash
python tools/build_midisc40.py
```

Writes:

- `~/Desktop/midisc 4.0.bin` — flash this
- `out/OCTATRACK_1.40MSC.syx` — MIDI upgrade image
- version string on device: **1.40MSC**

## Flash

1. Copy `midisc 4.0.bin` to the CompactFlash card root.
2. On the OT: **OS UPGRADE**.

### MIDI recovery / alternate install

Hold **FUNC** while powering on → option **3** → send `out/OCTATRACK_1.40MSC.syx` (or stock `OCTATRACK_OS1.40C.syx` to restore).

## Layout (Main OS caves)

| addr | region |
|------|--------|
| `0x400C45B0` | ENC_UNLOCK |
| `0x400D24D0` | SAFE_CAVE |
| `0x400D2EE6` | CAVE2 |
| `0x400D6500` | CODE2 |
| `0x400D6600` | MSC bank (`16×8×32`) |
| `0x400D7600` | STUB |
| `0x460C9A00` | DRAM CLIP / CKPT / lock list |

MSC index: `scene<<8 | track<<5 | flat`.

## Code

| path | role |
|------|------|
| `tools/midisc/memory_map.py` | addresses / hooks |
| `tools/midisc/parts.py` | pack / unpack / save / reload / clear |
| `tools/midisc/hold.py` | hold, dial, encoder unlock |
| `tools/midisc/morph.py` | XF mix, morph, plock |
| `tools/midisc/scene_ui.py` | scene clear / copy / paste |
| `tools/midisc/build.py` | link + patch Main OS |

Entry: `tools/build_midisc40.py`.
