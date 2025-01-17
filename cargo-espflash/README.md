# cargo-espflash

Cross-compiler and serial flasher cargo subcommand for Espressif SoCs and modules.

Currently supports the **ESP32**, **ESP32-C3**, **ESP32-S2**, **ESP32-S3**, and **ESP8266**.

Prior to flashing, the project is built using the `build-std` unstable Cargo feature. Please refer to the [cargo documentation] for more information.

[cargo documentation]: https://doc.rust-lang.org/cargo/reference/unstable.html#build-std

## Installation

```shell
$ cargo install cargo-espflash
```

Alternatively, you can use [cargo-binstall] to install pre-compiled binaries on any supported system. Please check the [releases] to see which architectures and operating systems have pre-compiled binaries.

```shell
$ cargo install cargo-binstall
$ cargo binstall cargo-espflash
```

[cargo-binstall]: https://github.com/ryankurte/cargo-binstall
[releases]: https://github.com/esp-rs/espflash/releases

## Usage

```text
cargo-espflash 1.7.0

USAGE:
    cargo espflash [OPTIONS] [SERIAL] [SUBCOMMAND]

ARGS:
    <SERIAL>    Serial port connected to target device

OPTIONS:
        --bin <BIN>
            Binary to build and flash

        --bootloader <BOOTLOADER>
            Path to a binary (.bin) bootloader file

        --erase-otadata
            Erase the OTADATA partition This is useful when using multiple OTA partitions and still
            wanting to be able to reflash via espflash

        --example <EXAMPLE>
            Example to build and flash

    -f, --flash-freq <FREQUENCY>
            Flash frequency [possible values: 12M, 15M, 16M, 20M, 24M, 26M, 30M, 40M, 48M, 60M, 80M]

        --features <FEATURES>
            Comma delimited list of build features

        --format <FORMAT>
            Image format to flash [possible values: bootloader, direct-boot]

        --frozen
            Require Cargo.lock and cache are up to date

    -h, --help
            Print help information

        --locked
            Require Cargo.lock is up to date

    -m, --flash-mode <MODE>
            Flash mode to use [possible values: QIO, QOUT, DIO, DOUT]

        --monitor
            Open a serial monitor after flashing

        --monitor-speed <MONITOR_SPEED>
            Baud rate at which to read console output

        --package <PACKAGE>
            Specify a (binary) package within a workspace to be built

        --partition-table <PARTITION_TABLE>
            Path to a CSV file containing partition table

        --ram
            Load the application to RAM instead of Flash

        --release
            Build the application using the release profile

    -s, --flash-size <SIZE>
            Flash size of the target [possible values: 256KB, 512KB, 1MB, 2MB, 4MB, 8MB, 16MB, 32MB,
            64MB, 128MB]

        --speed <SPEED>
            Baud rate at which to flash target device

        --target <TARGET>
            Target to build for

        --target-dir <TARGET_DIR>
            Directory for all generated artifacts

        --use-stub
            Use RAM stub for loading

    -V, --version
            Print version information

    -Z <UNSTABLE>
            Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details

SUBCOMMANDS:
    board-info         Display information about the connected board and exit without flashing
    help               Print this message or the help of the given subcommand(s)
    partition-table    Operations for partitions tables
    save-image         Save the image to disk instead of flashing to device
    serial-monitor     Open the serial monitor without flashing
```

## Configuration

You can also specify the serial port and/or expected VID/PID values by setting them in the configuration file. This file is in different locations depending on your operating system:

| Operating System | Configuration Path                                                       |
| :--------------- | :----------------------------------------------------------------------- |
| **Linux:**       | `/home/alice/.config/espflash/espflash.toml`                             |
| **Windows:**     | `C:\Users\Alice\AppData\Roaming\esp\espflash\espflash.toml`              |
| **macOS:**       | `/Users/Alice/Library/Application Support/rs.esp.espflash/espflash.toml` |

An example configuration file may look as follows (note that TOML does _not_ support hexadecimal literals):

```toml
[connection]
serial = "/dev/ttyUSB0"

[[usb_device]]
vid = "303A"
pid = "8000"
```

## WSL2

It is not possible to flash `usb-serial-jtag` chips with `WSL2` because the reset also resets `serial-jtag-peripheral` which disconnects the chip from WSL2.

## Package Metadata

You can specify the bootloader, partition table, or image format for a project in the package metadata in `Cargo.toml`:

```toml
[package.metadata.espflash]
partition_table = "partitions.csv"
bootloader = "bootloader.bin"
format = "direct-boot"
```

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](../LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](../LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in
the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without
any additional terms or conditions.
