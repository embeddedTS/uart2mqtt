# UART2MQTT

A minimal Python bridge that connects USB UART devices to MQTT topics. It automatically discovers USB serial devices, filters them using a VID\:PID allow list, and creates bidirectional communication between each serial port and its associated MQTT topic.

## Features

* Automatically detects serial devices via `/dev/serial/by-path`
* Filters devices using VID/PID from `/sys/class/tty/...`
* Bidirectional communication between UART and MQTT
* Dynamic topic generation: `<topic_base>/<port>/device_serial_output`
* Configurable via a TOML file at `/etc/uart2mqtt.toml`

## Configuration

Create `/etc/uart2mqtt.toml`:

```toml
[mqtt]
host = "mqtt-broker.local"
port = 1883
topic_base = "testbench"


# USB match sections are optional. If specified, it only matches with
# listed vid:pids. Can list [[usb_match]] multiple times, and can use "*" as 
# wilcards.
[[usb_match]]
vid = "1a86"
pid = "7523"

# If no usb_match is specified, all devices are matched similar to this:
[[usb_match]]
vid = "*"
pid = "*"
```

## Usage

Run the bridge:

```bash
uart2mqtt
```

See the example "uart2mqtt.service" example file as well.  Update it to include
the user this will run as, then:
```bash
cp /uart2mqtt.service /etc/systemd/system/uart2mqtt.service
systemctl --now enable uart2mqtt.service
```

## Requirements

* Python 3.11+
* `pyserial`
* `paho-mqtt`

## Development Setup:

After cloning, install the git pre-commit hooks.
```bash
# Run once:
pre-commit install

# To run by hand before a commit
pre-commit run --all-files
```

## License

MIT
