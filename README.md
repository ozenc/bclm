# BCLM

BCLM is a wrapper to read and write battery charge level max (BCLM) values to the System Management Controller (SMC) on Mac computers. This project was inspired by several battery management solutions, including Apple's own battery health management.

The purpose of limiting the battery's max charge is to prolong battery health and to prevent damage to the battery. Various sources show that the optimal charge range for operation of lithium-ion batteries is between 40% and 80%, commonly referred to as the 40-80 rule [[1]](https://www.apple.com/batteries/why-lithium-ion/)[[2]](https://www.eeworldonline.com/why-you-should-stop-fully-charging-your-smartphone-now/)[[3]](https://www.csmonitor.com/Technology/Tech/2014/0103/40-80-rule-New-tip-for-extending-battery-life). This project is especially helpful to people who leave their Macs on the charger all day, every day.

## Installation

The easiest method to install BCLM is through `brew`.

BCLM is written in Swift and is also trivial to compile. Currently, it can only be compiled on macOS Catalina (10.15) or higher but it can run on OS X Mavericks (10.9) or higher.

A release zip is also provided with a signed and notarized binary for those who do not have development tools or are on an older macOS version.

### Brew

```
$ brew tap zackelia/formulae
$ brew install bclm
```

### From Source

```
$ swift build
$ sudo swift test
$ cp .build/debug/bclm /usr/local/bin
```

### From Releases

```
$ unzip bclm.zip
$ cp bclm /usr/local/bin
```

## Usage

```
$ bclm
OVERVIEW: Battery Charge Level Max (BCLM) Utility.

USAGE: bclm <subcommand>

OPTIONS:
  --version               Show the version.
  -h, --help              Show help information.

SUBCOMMANDS:
  read                    Reads the BCLM value.
  write                   Writes a BCLM value.
  persist                 Persists bclm.
  unpersist               Unpersists bclm.

  See 'bclm help <subcommand>' for detailed help.
```

Note that in order to write values, the program must be run as root. This is not required for reading values.

## Persistence

The SMC can be reset by a startup shortcut or various other technical reasons. To ensure that the BCLM is always at its intended value, it should be persisted.

This will create a new plist in `/Library/LaunchDaemons` and load it via `launchctl`. It will persist with the current BCLM value and will update on subsequent BCLM writes.

```
$ sudo bclm persist
```

 Likewise, it can be unpersisted which will unload the plist.

```
$ sudo bclm unpersist
```
