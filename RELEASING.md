# Backup delivery playbook

This repository is a continuously updated K1 Max configuration backup. It has
no versioned releases; immutable Git commits are recovery points.

## Prepare and validate

Capture only intended printer configuration. Exclude credentials, network
identifiers, transient logs, caches, generated jobs, and unrelated runtime
state. Review the complete diff and validate syntax with the producing firmware
and Klipper versions. Record hardware modifications and firmware assumptions.

## Review, deliver, and verify

Use a pull request and squash merge to protected `main`; do not push backups
directly. Confirm CI passes and the merged files parse independently of the live
printer. Delivery does not authorize restoring them to hardware.

## Recover

Preserve current printer files before an authorized restore. Select a commit
compatible with the exact hardware and firmware, apply through the documented
recovery path, and verify heaters, axes, endstops, macros, and safety limits
before printing.

Fleet policy: <https://github.com/bolens/.github/blob/main/RELEASING.md>.

## Branch protection

The default branch requires pull requests, resolved conversations, linear
history, and an up-to-date branch with passing required checks, including `lint
/ actionlint` and `lint / zizmor`. These rules also apply to administrators;
force pushes and branch deletion are disabled. Zero approving reviews are
required because this is a solo-maintainer repository; review the complete diff
before merging.

Keep required checks available on every pull request. Filter expensive work
inside jobs or use an always-running result job that rejects failures and
cancellations. Update the protection settings when renaming required jobs.
