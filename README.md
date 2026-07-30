# Mainsail macros and settings (extended pause/cancel macros)

This fork extends the original `mainsail-config` macros with independent parking configuration for `PAUSE` and `CANCEL_PRINT`.


## Why?

The original configuration prohibits to configure `CANCEL_PRINT` to perform a Z lift without also performing an X/Y parking move. Additionally, while different X/Y parking positions can be configured for `PAUSE` and `CANCEL_PRINT`, the Z lift is shared, making it impossible to configure independent lift distances for the two commands.

This fork introduces a consistent configuration layout where each command has its own independent parking parameters:

* X position
* Y position
* Z lift

The goal is to achieve this simple configuration:

* **PAUSE**

  * lifts the nozzle by a small amount,
  * parks the toolhead at a predefined X/Y position,
  * allows convenient filament changes or inspection.

* **CANCEL_PRINT**

  * lifts the nozzle,
  * leaves the toolhead at its current X/Y position,
  * disables the motors and cooling fans.


## Compatibility

The macro functionality remains compatible with the original project and [fluidd-config](https://github.com/fluidd-core/fluidd-config) fork.

The only breaking change is the configuration interface: all user-defined parking variables have been renamed and reorganized into independent `pause_*` and `cancel_*` settings.

In order to reproduce original default behavior the user should set variables accordingly:

```ini
[gcode_macro _CLIENT_VARIABLE]
variable_park_at_pause_x     : None
variable_park_at_pause_y     : None
variable_park_at_pause_dz    : 2.0
variable_park_at_pause_z_min : 0.0
variable_pause_retract       : 1.0
variable_unretract           : 1.0

variable_park_at_cancel_x    : None
variable_park_at_cancel_y    : None
variable_park_at_cancel_dz   : 2.0
variable_cancel_retract      : 5.0
```

## Installation (updating from the original mainsail-config or fluidd-config)

1. Go to your Klipper configuration directory.

2. Backup the original macros (optional but recommended):

```bash
mv mainsail-config mainsail-config.backup
or
mv fluidd-config fluidd-config.backup
```

3. Clone this repository:

```bash
git clone https://github.com/laurvas/mainsail-config.git
```

4. Ensure that the `mainsail.cfg` symlink exists in the printer configuration directory:

```bash
ln -svf ~/mainsail-config/mainsail.cfg ~/printer_data/config/mainsail.cfg

```

5. Ensure that `mainsail.cfg` in included in `~/printer_data/config/printer.cfg`. If `fluidd.cfg` is currently included instead, replace it with `mainsail.cfg`.

6. Update your `_CLIENT_VARIABLE` configuration according to the new variable names described below. The old parking-related variables are no longer used and may be safely removed.

7. Restart Klipper.


## Example variables configuration

The following configuration parks the toolhead during `PAUSE`, while `CANCEL_PRINT` only raises the nozzle and keeps the current X/Y position.

```ini
[gcode_macro _CLIENT_VARIABLE]
variable_park_at_pause_x     : 1.0
variable_park_at_pause_y     : 1.0
variable_park_at_pause_dz    : 2.0
variable_park_at_pause_z_min : 50.0
variable_pause_retract       : 1.0
variable_unretract           : 1.0

variable_park_at_cancel_x    : None
variable_park_at_cancel_y    : None
variable_park_at_cancel_dz   : 30.0
variable_cancel_retract      : 5.0
```

See full variable list in `client.cfg`.
