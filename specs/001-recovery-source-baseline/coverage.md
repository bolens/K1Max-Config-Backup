# Requirement coverage

| Requirement | Source and acceptance evidence |
| --- | --- |
| FR-001 | `printer.cfg` printer section and include chain; static include inventory, not calibration approval. |
| FR-002 | Tracked file inventory and immutable inspected revision; retained snapshots and archives are unchanged. |
| FR-003 | Static include inventory records unavailable/symlinked GuppyScreen and Helper-Script targets without dereferencing external paths. |
| FR-004 | RELEASING.md recovery sequence and project-guide operational limits; no automatic restore or printer operation. |

## Verification receipt

Static inspection parsed 7 available files in the active include chain using Python RawConfigParser with interpolation disabled and identified the CoreXY kinematics. External helper targets were listed without following symlinks. This is structural INI evidence, not validation by the producing vendor Klipper firmware. Native integration status, JSON/Bash syntax, whitespace, actionlint, and offline zizmor checks passed. Separate self-review confirmed recovery-source ownership and explicit external/firmware limitations. No archive/database contents were unpacked and no hardware operation was performed.

## Detailed audit receipt: 2026-09-06

[Detailed contracts](legacy-contracts.md) cover 14 source families and all 46
available active macros. FR-005/FR-006 map to `tests/test_plot_files.py` and both
CLI readers/output paths; six file-boundary tests pass. FR-007 maps to
`tests/test_plot_numerics.py`; all five real numerical/PNG subcases pass. The
complete native gate passes 12 tests, Markdown/Ruff/source syntax and metadata
checks. Actionlint and offline Zizmor pass. Numerical fixtures do not validate
printer calibration, firmware commands or external helpers. Hosted delivery is
not yet complete.
