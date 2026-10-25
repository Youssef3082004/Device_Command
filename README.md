# Device

`device` is a lightweight command-line system monitor written in Bash. It gives you a quick snapshot of the current user, host, CPU load, memory consumption, disk utilization, and system uptime without starting an interactive monitoring tool.

## Features

- Displays the current username and hostname
- Reports CPU utilization from a one-time `top` snapshot
- Shows memory usage as a percentage
- Calculates both average filesystem usage and aggregate disk usage
- Prints human-readable system uptime
- Uses standard Linux command-line tools and has no runtime package manager

## Example output

```text
USERNAME: youssef
HOSTNAME: Vostro-15
CPU Usage: 9.3%
Memory Usage: 91.40%
Average Disk Usage: 13.94%
Disk Usage: 54.15%
UpTime: 6 hours, 4 minutes
```

Values vary according to the machine and its current workload.

## Requirements

- Linux
- Bash
- `bc`
- `procps` (`top` and `uptime`)
- GNU Coreutils (`df`)
- `awk`, `grep`, and `sed`

On Debian or Ubuntu, install the required packages with:

```bash
sudo apt update
sudo apt install bash bc procps coreutils gawk grep sed
```

## Installation

Clone the repository, then install the script somewhere on your `PATH`:

```bash
git clone https://github.com/Youssef3082004/Device_Command.git
cd Device
sudo install -m 0755 device /usr/local/bin/device
```

To install only for your user account:

```bash
mkdir -p "$HOME/.local/bin"
install -m 0755 device "$HOME/.local/bin/device"
```

Make sure `$HOME/.local/bin` is included in your `PATH`.

## Usage

After installation, run:

```bash
device
```

You can also run the script directly from the project directory without installing it:

```bash
bash device
```

The command does not require arguments or configuration.

## How it works

The script takes a non-interactive snapshot from `top` to derive CPU and memory values. It reads mounted filesystem statistics from `df`, performs percentage calculations with `bc`, and formats uptime using `uptime -p`.

The two disk figures answer different questions:

- **Average Disk Usage** is the arithmetic mean of the usage percentages reported for mounted filesystems.
- **Disk Usage** divides the total used blocks by the total available blocks across those filesystems.

## Compatibility notes

The parser expects the output format produced by GNU/Linux versions of `top`, `df`, and `uptime`. It is not currently designed for macOS, BSD, or localized command output. The memory calculation in the current implementation uses a 15 GiB reference value, so it should be adjusted before using the tool on systems with a different memory capacity.

## Development

Check the script for Bash syntax errors with:

```bash
bash -n device
```

Run it locally with:

```bash
bash device
```

When changing the output parsers, test on the Linux distributions you intend to support because system-tool output can differ between versions and locales.

## Contributing

Contributions are welcome. Fork the repository, create a focused branch, test your changes, and open a pull request that explains the problem and the proposed solution.

Useful areas for improvement include portable metric collection, argument handling, configurable output formats, and automated tests.
