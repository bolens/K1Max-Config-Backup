# Detailed recovery-source contracts

Inspected revision: `928b5da9577eaa0b928821307ffc8521dd6f3070`, 2026-09-06.
This records existing source after implementation. It does not certify a restore
bundle or hardware readiness. Original motion, thermal and calibration values
remain machine-specific recovery evidence.

| Contract | Owned source | Behavior and dependencies |
| --- | --- | --- |
| KR-001 | `printer.cfg`, `printer_params.cfg` | CoreXY configuration owns board/serial identities, motor pins/currents, axis bounds, extrusion/heaters, fans, accelerometer, vendor pressure-touch probing and saved calibration. Product parameters and operating limits are distinct; file header versions do not identify the complete producing firmware. |
| KR-002 | `sensorless.cfg` | Overrides homing with readiness/movement flags, forced initial moves, axis parking, repeated XY homing, Z centering and default mesh restoration. Relies on vendor print-state fields and actual sensorless hardware. Static parsing does not prove this sequence safe. |
| KR-003 | `gcode_macro.cfg` material/current macros | Load/unload helpers preserve G-code state, temporarily change fan/current settings, wait for extrusion temperature, move filament, then restore state/current. They depend on the configured extruder and external fan-control implementations. |
| KR-004 | `gcode_macro.cfg` Qmode/M204/M205/M107/M900 | Quiet mode saves motion/extrusion/fan state during printing or pause and later restores it. M204 clamps requested acceleration in quiet mode; M205 maps corner velocity; M107 selects fan shutdown; M900 maps pressure advance. These are retained firmware adapters, not general printer defaults. |
| KR-005 | `gcode_macro.cfg` print/pause/end/cancel macros | Coordinate preparation flags, extrusion guards, parking/lifting, reduced paused temperature, fan/current state, cooling waits and heater shutdown. START_PRINT/RESUME ownership partly resides in external helpers; commented definitions are historical alternatives, not active macros. |
| KR-006 | `gcode_macro.cfg` calibration macros | Delegate first-layer leveling, accurate homing, input shaping, bed PID and mesh calibration to firmware commands. Sensor names and vendor commands must resolve in the complete producing configuration before use. These macros are not run by repository validation. |
| KR-007 | `macros/test_speed.cfg` | Retains the imported homing/position-comparison and large/small motion-pattern test, including old/new cruise-limit compatibility. Its parameter and motion boundaries require firmware/operator verification; source presence is not permission to run it. |
| KR-008 | `GuppyScreen/guppy_cmd.cfg` | Exposes resonance/plot generation, belt-axis testing, sustained excitation, SSH-service command and material helpers. Shell paths, raw CSV names, external Guppy loader/config modules and firmware commands are runtime dependencies. Plot-file generation and physical resonance acquisition are separate operations. |
| KR-009 | `Helper-Script/KAMP/KAMP_Settings.cfg`, tracked symlinks | Settings store mesh/purge/park parameters; active includes refer to external helper implementations. Symlink targets are dependencies, not bundled file contents. Empty `variables.cfg` contains no captured runtime variable values. |
| KR-010 | `moonraker.conf`, `mobileraker.conf` | Capture server/socket/upload/history configuration, trusted-network authorization, update sources, camera and companion notification settings. Local endpoint values are machine state; no network reachability, authentication or service operation is established by parsing. |
| KR-011 | `GuppyScreen/scripts/calibrate_shaper.py`, `shaper_calibrate.py`, `shaper_defs.py` | Accept accelerometer/PSD data, combine frequency bins, normalize, estimate vibration/smoothing and rank configured shaper families. CLI outputs a plot and/or CSV plus a JSON result. Numerical models and recommended accelerations are estimates, not hardware guarantees. NumPy/Matplotlib and compatible firmware interfaces are required for their respective paths. |
| KR-012 | `GuppyScreen/scripts/graph_belts.py` | Compare two raw logs using PSD peaks, pairing, similarity and optional differential spectrogram. Report/plot experimental mechanical-health estimates. NumPy/Matplotlib are required; spectrograms additionally require SciPy. A supplied Klipper-directory option is currently unused. File-open waiting and producing data compatibility are runtime limits. |
| KR-013 | Hidden and dated printer/Moonraker snapshots | Preserve alternate recovery evidence independently of the active include roots. A file's presence does not mean the active configuration includes it. Select a compatible immutable commit before a separately authorized restore. |
| KR-014 | Development tooling, hooks and CI | Validate repository metadata, source hygiene and container-adapter behavior. These checks do not load the vendor firmware or operate the printer. Existing development-environment specification remains authoritative. |

## Findings still being resolved

Offline file-boundary regressions pass for raw CSV without comments, empty input
and preserving existing plots on failed saves. They execute the actual parsing
and CLI functions with substituted numerical/figure dependencies. Numerical
CLI execution now passes using installed system-Python packages and synthetic
data; the managed Python lacked those dependencies.

The retained G29 macro concatenates PROBE_COUNT without an equals sign; TEST_SPEED
reads SMALLPATTERNSIZE without the params prefix. Complete producing firmware and
external includes are unavailable, so those macro compatibility findings remain
open and no machine configuration has been changed.

## Acceptance limits

The static receipt below covers available includes, external symlinks, macro
definitions and structural snapshot parsing. Full configuration loading and
resolution of external macro references need the producing vendor firmware and
missing helpers. Homing, probing, heating, calibration acquisition and restoration remain
unperformed and require a separately scoped operator-controlled procedure.

## Active macro inventory

All 46 macro definitions in the seven available active include files map below.
External helper macros are not included in this count. `AUTOTUNE_SHAPERS` has an
empty body and supplies no tuning implementation. Parameter/state macros store
variables; the delayed cooling command and homing override are separate sections.

| Contract | Macro definitions |
| --- | --- |
| KR-001 | `PRINTER_PARAM`, `product_param` |
| KR-002 | `xyz_ready`, `_IF_HOME_Z`, `_IF_MOVE_XY`, `_HOME_X`, `_HOME_Y`, `_HOME_Z` |
| KR-003 | `LOAD_MATERIAL`, `LOAD_MATERIAL_CLOSE_FAN2`, `LOAD_MATERIAL_RESTORE_FAN2`, `QUIT_MATERIAL`, `RESTORE_E_CURRENT`, `SET_E_MIN_CURRENT` |
| KR-004 | `Qmode`, `Qmode_exit`, `M204`, `M205`, `M107`, `M900` |
| KR-005 | `CANCEL_PRINT`, `END_PRINT`, `END_PRINT_POINT`, `END_PRINT_POINT_WITHOUT_LIFTING`, `FIRST_FLOOR_PAUSE`, `FIRST_FLOOR_PAUSE_POSITION`, `FIRST_FLOOR_RESUME`, `PAUSE`, `PRINT_PREPARED`, `PRINT_PREPARE_CLEAR`, `WAIT_TEMP_END`, `WAIT_TEMP_START` |
| KR-006 | `ACCURATE_G28`, `AUTOTUNE_SHAPERS`, `BEDPID`, `G29`, `INPUTSHAPER`, `PRINT_CALIBRATION`, `TUNOFFINPUTSHAPER` |
| KR-007 | `TEST_SPEED` |
| KR-008 | `GUPPY_BELTS_SHAPER_CALIBRATION`, `GUPPY_EXCITATE_AXIS_AT_FREQ`, `GUPPY_SHAPERS`, `_GUPPY_LOAD_MATERIAL`, `_GUPPY_QUIT_MATERIAL` |
| KR-009 | `_KAMP_Settings` |

## Static and numerical receipt

The dated static audit parsed 13 regular configuration/snapshot files with
RawConfigParser and interpolation disabled, found seven available active include
files and 14 external helper symlinks, and reported no within-file structural
parse errors. Repeated sections across includes are not treated as a complete
firmware validation. Symlinks were enumerated without reading their targets.

Native validation passes 14 tests: five container-adapter tests, eight file-boundary
tests, and one numerical test with five CLI subcases. Synthetic captures render
shaper plots at 100/200/300 Hz, belt comparison and a differential spectrogram.
The system Python provides the numerical packages; the locked development shell
now declares the same package families. No hardware data acquisition occurred.

The initial check hit ENOSPC while writing caches despite later filesystem free
space. Validation passed using cache/bytecode directories under `/tmp`. Hosted
Nix/container verification and delivery remain pending. Local Nix syntax parsing
passes. The plot replacement guarantee covers image output; calibration CSV
writing retains its existing behavior.

FR-008 maps to the explicit belt `--offline` option and two capture-wait
regressions. Completed synthetic captures use this mode on both platforms.
The default still waits on Linux process descriptors. The first macOS run
exposed the Linux-only check; hosted validation of this correction is pending.
