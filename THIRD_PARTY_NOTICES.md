# Third-party notices

## License scope

The root MIT license covers original material authored by bolens. It does not
replace third-party licenses, copyright notices, trademarks, or service terms.
Imported and modified third-party material keeps its applicable upstream terms.

## Printer firmware and screen material

`GuppyScreen/scripts/calibrate_shaper.py`, `shaper_calibrate.py`, and
`shaper_defs.py` retain Klipper copyright headers for Dmitry Butyugin and Kevin
O'Connor and their GPLv3 terms. The full GPL is bundled below. The retrieved GuppyScreen
license is also GPLv3. Preserve source headers and mark subsequent modifications.

`macros/test_speed.cfg` credits
[Andrew Ellis's TEST_SPEED macro](https://github.com/AndrewEllis93/Print-Tuning-Guide/blob/bef55254f88044149478561e6f6e7cea3d1aa352/macros/TEST_SPEED.cfg).
The audited upstream tree has no license file. Attribution alone does not grant
redistribution rights. This macro is excluded from MIT pending permission or
verified historical licensing.

Factory configuration, copied macros, and recovery snapshots are also excluded
from the root MIT grant until their exact provenance and terms are established.

## GitHub Spec Kit

Imported `.specify/scripts/`, `.specify/templates/`, and
`.agents/skills/speckit-*` integration files retain GitHub's MIT copyright and
permission notice in [.specify/LICENSE](.specify/LICENSE). Include it when
copying these files. Project-authored memory documents have separate ownership.

## Retained upstream license copies

These source URLs identify the retrieved license text. They do not establish
the exact revision of older unrecorded imports. File-level notices take
precedence over a project-wide license.

- **klipper**: `GuppyScreen/`. [Upstream license](https://github.com/Klipper3d/klipper/blob/f0892d82b0f1c1228454f09eb508eddde2250f4b/COPYING). Full copy: [LICENSES/klipper.txt](LICENSES/klipper.txt).
- **guppyscreen**: `GuppyScreen/`. [Upstream license](https://github.com/ballaswag/guppyscreen/blob/07409cb031bbbfc57cd7817ba295e5385e3d5565/LICENSE). Full copy: [LICENSES/guppyscreen.txt](LICENSES/guppyscreen.txt).

## Redistribution

Keep applicable full license and copyright notices with copied source and
bundled dependencies, including minified JavaScript and compiled executables.
Use the exact dependency versions selected by the lockfile or build. Preserve
Apache NOTICE material and satisfy copyleft source requirements where they
apply. Development-only tools and separately installed programs keep their own
terms but are not automatically part of a distributed application.

This source inventory is not proof that every historical release, external
asset, fetched dataset, or built container has satisfied its license obligations.
