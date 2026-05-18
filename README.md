# DSB Builder

A Claude skill that builds Mitsubishi Diamond System Builder (DSB) projects from a phone photo of a handwritten Field Survey Checklist. Output: a saved `.dsbx` project file and an exported PDF submittal, dropped back to the contractor's phone via chat.

> Built by [Rizvan Fazli](https://github.com/iTzN0VQ) — Mitsubishi-certified lead HVAC tech.
> Part of [NovaCTRL.ai](https://novactrl.ai).

---

## What it does

You hand a tech a 1-page Field Survey Checklist. They fill it out at the kitchen table, snap a photo with their phone, drop it in the chat. The skill:

1. **Validates the form** — every required field, contradiction-free. If anything's blank or unreadable, it tells the tech exactly what to fix instead of guessing.
2. **Drives DSB on a Windows machine** via Claude's `computer-use` tooling — Project Properties, Outdoor Unit, Branch Box, every Indoor Unit, every controller.
3. **Handles the R454B floor-area gate, M-NET defaults, BB end-caps, controller routing** — including the per-zone Kumo / MHK2 / PAR-42 / PAC-445 logic that's easy to miss when you're rushing a submittal.
4. **Exports a saved `.dsbx` and a PDF submittal** to the folder the form specifies.
5. **Attaches the PDF back to the chat** — the contractor opens it on their phone.

Saves 20–30 minutes per submittal. More importantly: makes the submittal happen at all on jobs where the tech would otherwise have to wait until they got back to a laptop.

---

## How to install

This is a Claude skill (a single `SKILL.md` file with YAML frontmatter). It runs in:

- **Claude Code** — drop `SKILL.md` into `~/.claude/skills/dsb-builder/` (or a project-level `skills/` folder).
- **Cowork mode** in the Claude desktop app — install as a personal or workspace skill.
- **Any Claude product that supports the SKILL.md format.**

Once installed, trigger the skill by uploading a photo of a filled-out Field Survey Checklist with any of these phrases: "build the DSB", "spin up DSB", "run the submittal", "do the worksheet".

---

## Requirements

To actually drive DSB end-to-end the runner machine needs:

- **Windows** with **Mitsubishi Diamond System Builder** installed (the desktop app — free download from [mitsubishicomfort.com](https://www.mitsubishicomfort.com/dsb)).
- **Claude with `computer-use`** access granted to Diamond System Builder, File Explorer, and (optionally) Google Chrome.
- **The Field Survey Checklist** — see below.

The skill assumes R454B current-generation equipment (M-Series single / MXZ multi / MXZ-SM / P-Series), capped at 8 indoor heads. For City Multi jobs use a different sheet.

---

## The Field Survey Checklist

The skill consumes a 2-page checklist organized into 5 sections:

| § | Section            | Key fields                                                                                              |
|---|--------------------|---------------------------------------------------------------------------------------------------------|
| 1 | Project Info       | Project name (→ save filename), Customer, Address, Date, Tech, Job # (→ Cont. Number)                   |
| 2 | Outdoor Unit       | Family, Capacity (kBTU), Hyper-Heat, Voltage, Phase, OD elevation, Mount, Service clearance             |
| 3 | Branch Box         | No-BB checkbox, BB ports, Run OD→BB, # of 90s, BB elevation                                             |
| 4 | Output / DSB File  | Save filename, Save folder, Output checkboxes (equip schedule / lineset summary / PDF / email)          |
| 5 | Indoor Units       | Up to 8 blocks: Zone/Room, Type, Cap, Mount ht, Lineset, # of 90s, Vert +/-, Floor area, Controller     |

---

## What it does NOT do

- **It does not redesign systems.** If DSB says "Total connectable capacity exceeds outdoor unit limit" or any other design-time error, the skill stops, names the conflict, and waits for the tech to resubmit a fixed form. Design choices belong to the tech, not the agent.
- **It does not bypass M-NET.** Never switches to non-M-NET to make a model show up.
- **It does not handle City Multi.** Use the City Multi survey sheet instead.

---

## Equipment cheat sheet (R454B)

| Form term                        | DSB type                       | Model                            |
|----------------------------------|--------------------------------|----------------------------------|
| MXZ-SM 36K 1ph (no HH)           | OD                             | `MXZ-SM36NL`                     |
| MXZ-SM 48K 1ph (no HH)           | OD                             | `MXZ-SM48NL`                     |
| MXZ-SM 48K 1ph HH                | OD                             | `MXZ-SM48NLHZ`                   |
| 3-port branch box                | BB                             | `PAC-LMA30BC`                    |
| 5-port branch box                | BB                             | `PAC-LMA50BC`                    |
| 4-way cassette 18/24/30/36       | Ceiling-Cassette (Four-Way)    | `PLA-AE##NL`                     |
| 1-way cassette                   | Ceiling Cassette (One-Way)     | `SLZ-AF##NL`                     |
| AHU 12/18/24/30/36               | Multi Position                 | `SVZ-AP##NL`                     |
| PAR-42 wired                     | MA Remote Controller           | `PAR-42MAAUB`                    |
| MHK2 wireless                    | IDU Accessory                  | MHK2 RedLink kit                 |
| Kumo Cloud                       | IDU Accessory                  | `PAC-USWHS002-WF-2`              |
| PAC-445 + 3rd-party stat         | IDU Accessory                  | `PAC-US445CN-1`                  |

Full procedural detail (UI quirks, error handling, controller routing, save/export sequence) lives in [SKILL.md](./SKILL.md).

---

## License

MIT — see [LICENSE](./LICENSE). Use it, fork it, improve it. If you ship something better, ping me on [novactrl.ai](https://novactrl.ai).

---

## Author

Rizvan Fazli · rizvanfazli8@gmail.com · [NovaCTRL.ai](https://novactrl.ai)
