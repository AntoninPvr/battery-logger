# Battery Logger Script

## Objective

This script was designed to log battery information from a system, originally for the **Razer Blade 15 Advanced (2020)** but it can support almost all laptops or battery powered linux systems. It provides periodic logging of battery parameters such as current, voltage, capacity, charge, temperature, and charging status. The script is flexible, allowing you to specify logging intervals, battery paths, output files, and maximum runtime. Additionally, it includes an optional terminal interface to display live data and log size.

## Features
- Logs battery data to a CSV file.
- Allows specification of output file and interval.
- Option to specify a custom battery path.
- Maximum runtime option (runs indefinitely by default).
- Display a real-time terminal interface with battery data.
- Supports both command-line arguments and help options.

## Prerequisites
- Linux system with `/sys/class/power_supply/` for battery information.
- `acpi` command installed (for temperature reading, optional).
- Bash shell.

---

## Installation

```bash
chmod +x battery_logger.sh
```

## Usage

```bash
./battery_logger.sh [OPTIONS]
```
### Options:

- `-o, --output <FILE>`  
  Specify the output CSV file where the battery data will be logged (default: `battery_log.csv`).

- `-i, --interval <SECONDS>`  
  Specify the logging interval in seconds (default: `60`).

- `-b, --battery <BATTERY_PATH>`  
  Specify the path to the battery information (default: autodetected as `/sys/class/power_supply/BAT*` and take the first occurence). 

- `-t, --time <SECONDS>`  
  Specify the maximum runtime of the script in seconds. If set to `0`, the script runs indefinitely.

- `--no-interface`  
  Disable the terminal interface that displays real-time battery data (default: enabled).

- `-h, --help`  
  Display the help message.

To exit the script, press `Ctrl+C`.

### Example:
    
- To log **first battery** data every **30 seconds** to a file named `battery_0_log.csv` indefinitely:

    ```bash
    ./battery_logger.sh -o battery_0_log.csv -i 30
    ```

- To log **second battery** data every **2 seconds** to a file named `my_battery_log.csv` for 1 hour:

    ```bash
    ./battery_logger.sh -o my_battery_log.csv -i 2 -b /sys/class/power_supply/BAT1 -t 3600
    ```

### Terminal Interface:

The terminal interface displays real-time battery data and log file size. It is enabled by default and can be disabled using the `--no-interface` option.

```
====================== BATTERY LOGGER =====================
Interval: 10 seconds
Battery Path: /sys/class/power_supply/BAT0
Start Time: Fri Dec  6 07:31:27 PM EST 2024
===========================================================
Log File: battery_log.csv
Current Log File Size: 4.0K

=================== Current Battery Data ==================
| Metric         | Value (converted)  | Raw Value         |
|----------------|--------------------|-------------------|
| current_now    | 1089.000 mA        | 1089000           |
| charge_now     | 4073.000 mAh       | 4073000           |
| voltage_now    | 16.071 V           | 16071000          |
| capacity       | 75 %                                   |
===========================================================
temperature      : 27.8 °C
charging status  : Discharging
===========================================================
Progress: |###########                      | ETA: 01:36:21
```

## Notes

- **current_now** : Current in microamperes.
    some systems may report only positive values. In this case, charging status can be used to determine the direction of the current.
- **charge_now** : Charge in microampere-hours.
- **capacity** : Battery capacity in percentage.
- **voltage_now** : Voltage in microvolts.
- **temperature** : Battery temperature in Celsius.
- **charging status** :

  | Status | Value |
  |---|---|
  | Charging | 1 |
  | Discharging | -1 |
  | Full | 0 |
  | Unknown | 0 |

## Known Issues

- Battery temperature : on some system battery temperature is always 27.8°C.