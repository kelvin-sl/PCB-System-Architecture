# PCB System Architecture — Cadence Connectivity

[繁體中文](README.md) | **English**

**A step-by-step workflow for schematic and PCB collaboration, backed by traceable evidence.**

[MIT License](LICENSE) · [Installation and usage](#quick-start) · [Contributing](CONTRIBUTING.md)

`cadence-connectivity` is an instruction-based Codex skill for OrCAD Capture Tcl and Allegro PCB Editor SKILL. It guides collaboration from inspecting the current design and planning signal mappings through change previews, execution, readback, and verification.

This README is available in English. The skill instructions, reference guides, contribution guide, and changelog are currently written in Traditional Chinese. Ask Codex to respond in English when using the skill, as shown below.

## What it helps with

| Task | Guidance provided |
| --- | --- |
| Schematic connectivity | Fixed connector pinouts, signal mapping, off-page attachment, polarity, replacement of existing connections, and label placement |
| Reusing an existing board | Auditing RefDes, netlists, existing placement and routing, and recording changes |
| Placement | Shared baseline versions, coordinate transforms, footprint origins, Pin 1, board outlines, and Keepin boundaries |
| Incremental routing | Endpoints, surrounding obstacles, effective clearance rules, existing traces and vias, and staged verification |
| Execution and recovery | Script and report versions, manual or authorized execution, readback, stopping on failure, and evidence of rollback |

The repository contains workflow instructions and reference documents. It does not include an MCP server, a CAD execution agent, a general-purpose autorouter, or project design files. Scripts must be generated and verified for the actual design and tool version.

## Quick start

### 1. Install the skill

In a Codex environment that provides `$skill-installer`, paste:

```text
$skill-installer
Install the skill from https://github.com/kelvin-sl/PCB-System-Architecture.
SKILL.md is at the repository root (path: .).
Use cadence-connectivity as the installed skill name.
If a skill with that name already exists, compare versions and local changes
before replacing anything.
```

Alternatively, select **Code → Download ZIP** on GitHub. Extract the archive and rename the folder containing `SKILL.md`, `agents/`, and `references/` to `cadence-connectivity`. Place that entire folder in your Codex skill directory, keeping the license and copyright files.

The official documentation lists `~/.agents/skills/cadence-connectivity` as a user-level location. If your existing environment uses a different installation location, follow its installer. Avoid installing duplicate copies with the same skill name.

On your next turn, check that `$cadence-connectivity` is available. If it does not appear, restart Codex and check again. See the [official OpenAI skills documentation](https://learn.chatgpt.com/docs/build-skills) for skill format and discovery details.

### 2. Describe the current state and fixed constraints

Provide the CAD tool and version, the design scope, the pinout/bank/polarity assignments that must remain fixed, and the reports available for inspection. Keep project data in your own project directory.

For a new mapping task:

```text
$cadence-connectivity
Please respond in English throughout this task.
I want to connect existing connector signals to a designated FPGA bank.
The external connector pinout is fixed. Assignments within the bank may change,
but differential P/N polarity must be preserved.
First prepare a read-only audit from the available information, identify
existing signal assignments and missing evidence, then propose an initial mapping.
For this turn, perform analysis and preview only.
```

To continue after running a script:

```text
$cadence-connectivity
Please respond in English throughout this task.
Here is the complete output from the previous script.
Check that it corresponds to the current design and script version.
Separate passed, failed, and unverified items, then continue with the next
step already authorized for this task.
```

### 3. Advance using evidence

```text
Inspect current state → Confirm constraints and mapping → Preview changes
    → Execute → Read back the expected postconditions

On failure: stop and verify the scope of recovery before continuing.

Track electrical, routing, mechanical, and saved-file verification separately.
```

A successful tool return supports only that operation's result. Pin/net assignment, physical copper connectivity, design rule checks (DRC), signal integrity (SI), mechanical fit, and saved design state each need their own evidence.

## Documentation map

The following documents are currently in Traditional Chinese:

- [SKILL.md](SKILL.md): agent entry point and the standard workflow.
- [Capture](references/capture.md): Tcl, object identification, off-page connectors, and text display.
- [Allegro](references/allegro.md): physical pinout exports and placement interpretation.
- [Mapping and change planning](references/planning.md): fixed endpoints, existing assignments, and change lists.
- [Placement and boundaries](references/placement.md): coordinates, mating geometry, shared baselines, board outlines, and Keepin.
- [Incremental routing](references/routing.md): geometry, rules, existing paths, and verification.
- [Execution channels](references/execution.md): local/remote file roundtrips, triggering, and result identification.
- [Verification and recovery](references/verification.md): output checks, failures, and recovery limits.
- [Board reuse](references/board-reuse.md): workflows for modifying an existing design.
- [Project records](references/project-data.md): private data, change records, and deliverables.

## Compatibility and verification scope

Reading or installing this skill does not require a Cadence runtime. Running generated Tcl or SKILL scripts requires the appropriate software, licenses, design access, and an available execution method.

Checks performed so far cover document format, references, public content, and synthetic workflow scenarios for placement, routing, rollback, file roundtrips, and publication boundaries. They do not certify API compatibility across Cadence versions or guarantee that a particular design passes electrical rule checks (ERC), DRC, signal integrity, or manufacturing verification.

## Contributions and issue reports

Read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting changes. Reproducible improvements to general workflows are welcome. Use synthetic or reviewed, de-identified examples in public issues and pull requests. Do not upload real board files, pin tables, full runtime logs, internal paths, or credentials.

The [changelog](CHANGELOG.md) tracks public skill changes. Actual board-level progress belongs in each user's private project records.

## License and affiliation

Repository content is provided under the [MIT License](LICENSE), with the copyright notice `Copyright (c) 2026 kelvin-sl`. Retain the copyright and permission notice when using, modifying, or redistributing the content. The complete terms are in LICENSE; the standard text is also available from the [Open Source Initiative](https://opensource.org/license/mit).

This license covers the repository's content. It does not grant rights to Cadence software, third-party libraries, or users' private designs.

This is an independent community workflow. It does not claim official certification, sponsorship, or endorsement by Cadence or OpenAI. Product names identify the applicable tools; the respective rights holders retain their rights to software, trademarks, and third-party materials.
