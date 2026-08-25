# Tecotec Sonar Program — Concept & CONOPS Synthesis

Filed from the Tecotec LLM Wiki (`obs_vault/Wiki/`) on 2026-08-25. Query: "tecotec sonar program concept con-op".

## Direct answer

**Tecotec's sonar program has no documented CONOPS.** The program's root working note has an empty "II. CON-OP" section that was never filled in.

## The program

`SYS-SONAR-17` — Tecotec's sonar system development program, started 2017, pilot phase 2025–2027. Scope per the wiki entity page:

- Passive sonar array (hydrophone array + signal processing)
- Active sonar transmit (waveform generation, power amplifier)
- Pinger / ULB supply
- Oceanographic monitoring (Argo floats, moorings, SVP)
- Underwater acoustic comms (JANUS/STANAG 4748, ELAC UT3000)
- Acoustic measurement services (hydrophone calibration, uncertainty budget)

Root working note: `TECOTEC Sonar program.md` (currently in the vault's `.trash` — moved out of the live vault, not deleted). Its "KNOWLEDGE BREAKDOWN" outlines five planned sections — **Specifications, CON-OP, Trade-off, Environment Problem, Array** — and only *Array* has any content (oil-filled tube, pressure-compensated; radio-telescope-style deployment ideas noted as unprocessed). CON-OP was never written.

## The nearest thing to a CONOPS in the wiki

Not from the sonar program itself — from an adjacent research thread, the **Underwater SIGINT** concept page (ingested 2026-07-30), which closes with a 5-tier CONOPS framework directly applicable to a sonar/ACINT concept of operations:

1. **Strategic fixed infrastructure** — bottom-mounted hydrophone arrays, cable-tap programs, VLF/ELF shore stations, satellite ocean surveillance.
2. **Tactical mobile platforms** — submarine SIGINT missions, towed arrays (SURTASS), sonobuoys, persistent USV surveillance.
3. **Unmanned distributed networks** — UUV-deployed acoustic arrays, underwater unattended ground sensors, air-deployable UUV swarms, multi-static active/passive networks.
4. **Cross-domain integration** — air-to-underwater eavesdropping, space-based ocean surveillance feeding underwater ASW, fused ACINT+ELINT+COMINT+MASINT picture.
5. **Intelligence exploitation** — signature-library management, movement-pattern analysis, traffic analysis, order-of-battle construction.

Also relevant: a Con-Op research item exists for `SYS-USV-25` ("research new USV concept of operation – 2026," status: Done) but its content was never captured into the wiki — a dead-end pointer, not usable material.

## Strategic/market framing that would feed a CONOPS

From "Sonar R&D Global Landscape — From Basic Science to Productization" (2026-06-17): the **sonobuoy program** is flagged as the highest-leverage development target (STANAG 4748/JANUS interface, India-US JV co-production precedent). Vietnam's strategic gaps: no domestic sonobuoy production, no towed array passive sonar for surface combatants, no national SVP monitoring network, and nascent acoustic signal processing software capability.

The CSIS 3-phase SEA undersea-awareness framework (sensor deployment → data integration → capacity building) is cited as direct demand justification for Tecotec-class systems.

## Gaps / open questions

- **No documented, program-specific sonar CONOPS exists in the wiki.** This is a coverage gap, not a contradiction between sources.
- If a sonar CONOPS document exists outside the vault (e.g. under `WORK\02 RnD programs\Underwater\`), it has not been ingested into the wiki.
- `HW-SNR-26` as a project code does not match any existing wiki entity — the live sonar entity is `sys-sonar-17` (system-level, hw+sw, started 2017, per the `hw-/sw-/sys-/serv-/dt-` naming convention). Whether `HW-SNR-26` represents a deliberate rename/rescope (hardware-only, restarted 2026) or a separate initiative is a naming/scoping decision for the program owner, not something the wiki can resolve unassisted.

## Sources cited

- Wiki entity: `entities/projects/sys-sonar-17`
- Wiki source: `sources/2026-06-10-tecotec-underwater-program`
- Wiki source: `sources/2026-06-17-sonar-rd-global-landscape`
- Wiki source: `sources/2026-06-17-sonar-system-bom-supplier-database`
- Wiki source: `sources/2026-06-15-usv-master-plan`
- Wiki concept: `concepts/underwater-sigint`
- Wiki source: `sources/2026-07-30-underwater-sigint-deep-brainstorm`
- Vault root note (trashed): `.trash/TECOTEC Sonar program.md`
- Naming convention: `Wiki/entities/projects/_naming-convention.md`
