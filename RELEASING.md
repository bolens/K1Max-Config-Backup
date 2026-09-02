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
