# Mainsail klipper macros and settings (extended pause/cancel macros)

This fork extends the original `mainsail-config` macros by allowing **independent parking behavior** for `PAUSE` and `CANCEL_PRINT`.

## Why?

The original macros assume that `PAUSE` and `CANCEL_PRINT` should use the same parking behavior. In practice these commands serve different purposes and often require different parking strategies.

For example, on some printers it is desirable that:

* **PAUSE**

  * lifts the nozzle by a small amount,
  * parks the toolhead at a predefined X/Y position,
  * allows convenient filament changes or inspection.

* **CANCEL_PRINT**

  * only lifts the nozzle,
  * leaves the toolhead at its current X/Y position,
  * disables the motors (or performs any custom shutdown sequence).

This behavior cannot be configured using the original macros because both commands use the same parking parameters.

## What's changed?

The parking configuration has been split into separate settings for each command.

`PAUSE` and `CANCEL_PRINT` now have independent:

* X position
* Y position
* Z lift

This allows completely different parking strategies for each command.

Example:

| Command      | X/Y parking               | Z lift |
| ------------ | ------------------------- | ------ |
| PAUSE        | Park at rear-right corner | 2 mm   |
| CANCEL_PRINT | Keep current X/Y position | 20 mm  |


## Compatibility

This fork preserves the original macro behavior while introducing separate parking configuration for PAUSE and CANCEL_PRINT.

The only incompatible change is the configuration format: parking-related variables have been renamed and split into independent `pause_*` and `cancel_*` settings. Using identical values for both commands reproduces the original behavior.


## Example configuration

```ini
[gcode_macro _CLIENT_VARIABLE]
variable_park_at_pause_x     : 0.0
variable_park_at_pause_y     : 0.0
variable_park_at_pause_dz    : 2.0
variable_park_at_pause_z_min : 0.0
variable_pause_retract       : 1.0
variable_unretract           : 1.0

variable_park_at_cancel_x    : None
variable_park_at_cancel_y    : None
variable_park_at_cancel_dz   : 30.0
variable_cancel_retract      : 5.0
```
