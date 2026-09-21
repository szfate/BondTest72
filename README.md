# BondTest72

Production test fixture board for bond wire testing, controlled by a Raspberry Pi Pico.
Test signals are routed through CH446X analog crosspoint switches to two 41-pin connectors, on which a replaceable adapter board sits. The adapter board carries the DUT connector and any auxiliary components, keeping the main tester board from wearing out.

![Board render](docs/board-render.png)

## Specifications

| Parameter | Value |
|-----------|-------|
| Revision | r2 |
| Date | 2026-09-20 |
| Board size | 80 × 90 mm |
| Layers | 4 (F.Cu / In1.Cu / In2.Cu / B.Cu) |
| Thickness | 1.6 mm FR4 |


## Power

The board is powered from the Pico's USB VBUS rail. The TPS7A0533 LDO provides an optional quiet +3.3VADC rail as an alternative ADC reference, in case the main +3.3V rail from the Pico's onboard DCDC is too noisy. In practice the application only requires ~0.1 V accuracy, so the main rail is likely sufficient.

An ICL7660 charge-pump voltage inverter (or equivalent) generates the negative VEE rail required by the CH446X analog crosspoint switches.

## Operator Interface

A single push button serves as the START control. The two SK6812 RGB LEDs serve as status indicators. Both the LEDs and the button sit on the right edge of the board so the prodcution panel cannot cover them.

## Signal Routing

Four CH446X analog crosspoint switches fan test signals out from the Pico GPIO/ADC lines to the 41-pin connectors (J3 / J4). These connect to a removable **adapter board** rather than directly to the DUT. The adapter board carries the DUT-side connector (the high-wear part) and any auxiliary components needed for a given test configuration — for example, diodes for self-testing or a small memory chip for wear-cycle monitoring. Replacing the adapter board instead of the whole fixture avoids discarding the tester after the DUT connector wears out (~100 cycles). Solder jumpers on the main board provide flexibility to work around any schematic design flaws discovered after fabrication.

## Revision History

### r2 (2026-09-21)

Schematic:
- Neighbour testing removed, replaced with 10 µA / 100 µA / 1 mA current sources; pullup resistors (280 kΩ / 27.4 kΩ / 2.49 kΩ) sized from the expected diode voltage drop at each current
- ICL7660 (or alternative) added for the negative VEE supply
- 100 kΩ pulldown on Pico pad 22 (CON6, EEPROM SIO)
- 10 kΩ pullup on Pico pad 1 (START) per the RP2350 A2 errata
- 120 pF capacitor added to the ADC sensing net
- Status LEDs reduced from three to two SK6812
- Push button changed to EVP-BT3C4A000; r1's alternate second switch footprint removed

PCB:
- Board size reduced from 80 × 100 mm to 80 × 90 mm
- LEDs and button moved to the right side of the board so the panel no longer covers the LEDs; with the board rotated 90° the LEDs face front. Space left for button caps
- Feet marks on the bottom silk enlarged from 8 mm to 10 mm

### r1 (2026-05-12)

Initial release. 80 × 100 mm.

## Files

| File | Description |
|------|-------------|
| `BondTest72.kicad_pro` | KiCad project |
| `BondTest72.kicad_sch` | Schematic |
| `BondTest72.kicad_pcb` | PCB layout |
| `docs/BondTest72-r2-schematic.pdf` | r2 schematic PDF export |
| `docs/BondTest72-r1-schematic.pdf` | r1 schematic PDF export |
| `production/BondTest72-r2.zip` | r2 gerber/fabrication outputs |
| `production/BondTest72-r2-bom.csv` | r2 bill of materials |
| `production/BondTest72-r2-pos.xlsx` | r2 pick-and-place positions |
| `lib.kicad_sym` | Project symbol library |
| `lib.pretty/` | Project footprint library |
| `df9.pretty/` | Additional footprint library |
| `docs/` | Board renders and documentation images |
