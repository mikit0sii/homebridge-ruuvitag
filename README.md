# homebridge-ruuvitag

With this [Homebridge](https://github.com/homebridge/homebridge) plugin you can use [RuuviTags](https://tag.ruuvi.com/) with [Apple HomeKit](https://www.apple.com/ios/home/).

> ⚠️ **Homebridge 2.x / Node 22 users** — the plugin loads but the native BLE dependency (`@abandonware/bluetooth-hci-socket`) ships a prebuilt binary compiled for an older Node ABI. Follow the [Fix for Homebridge 2.x / Node 22](#fix-for-homebridge-2x--node-22-raspberry-pi) section below before doing anything else.

## Updates
- 5.0.0: Added ruuvitag version 5 support
- 2.3.0: Updated ruuvitag support
- 1.8.0: Fixed flooding issue and added `frequency` (update frequency) parameter
- 1.7.0: Added support for latest Node.js versions!
- 1.5.0: [Humidity triggers!](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.5.0)
- 1.4.0: [Disable temp/humidity](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.4.0)
- 1.3.1: [Enhanced movement formula](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.1)
- 1.3.0: [Motion triggers!](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.0)
- 1.2.0: [You can now set up heat and cold triggers](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.2.0)
- 1.1.0: [Show battery level + low battery warning](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.1.0)

---

## Fix for Homebridge 2.x / Node 22 (Raspberry Pi)

### Symptoms

If you see either of these errors in your Homebridge logs, follow this fix:

```
TypeError: Service.BatteryService is not a constructor
```
```
Error: The module '.../@abandonware/bluetooth-hci-socket/build/Release/bluetooth_hci_socket.node'
was compiled against a different Node.js version using NODE_MODULE_VERSION 127.
This version of Node.js requires NODE_MODULE_VERSION 137.
```

### Root cause

Homebridge 2.x requires Node.js ≥ 22.12.0 (enforced — you cannot downgrade). The plugin's BLE stack (`@abandonware/bluetooth-hci-socket`) ships a prebuilt binary compiled for an older Node ABI (127 = Node 18), and its `binding.gyp` is incompatible with `node-gyp` 10.x so it cannot be recompiled. The solution is to replace it with the maintained [`@stoprocent/bluetooth-hci-socket`](https://www.npmjs.com/package/@stoprocent/bluetooth-hci-socket) fork, which supports Node 22 and compiles cleanly.

### Step 1 — Install system build dependencies

```bash
sudo apt-get install -y build-essential libbluetooth-dev libudev-dev
```

### Step 2 — Install and compile the replacement BLE binding

```bash
cd /var/lib/homebridge/node_modules/homebridge-ruuvitag

# Install the Node 22-compatible BLE fork
sudo npm install @stoprocent/bluetooth-hci-socket --build-from-source

# Build it (in case npm reused a cached prebuilt)
cd node_modules/@stoprocent/bluetooth-hci-socket
sudo /var/lib/homebridge/node_modules/homebridge-ruuvitag/node_modules/.bin/node-gyp rebuild

# Verify the binary was produced
ls build/Release/bluetooth_hci_socket.node
```

You should see `build/Release/bluetooth_hci_socket.node` listed.

### Step 3 — Replace the broken binary

```bash
sudo cp \
  /var/lib/homebridge/node_modules/homebridge-ruuvitag/node_modules/@stoprocent/bluetooth-hci-socket/build/Release/bluetooth_hci_socket.node \
  /var/lib/homebridge/node_modules/homebridge-ruuvitag/node_modules/@abandonware/bluetooth-hci-socket/build/Release/bluetooth_hci_socket.node
```

### Step 4 — Restart Homebridge

```bash
sudo hb-service restart
```

### Step 5 — Verify

In Homebridge logs you should now see:

```
Loaded plugin: homebridge-ruuvitag@5.2.0
Registering accessory 'homebridge-ruuvitag.Ruuvitag'
```

...with no errors, and your RuuviTag accessories initializing normally.

> **Note:** After Homebridge re-registers the accessories for the first time, HomeKit will place all sensors in the Default Room. Simply open the **Apple Home** app, long-press each sensor → Settings → Room, and reassign them.

---

## Standard Installation

First, install [Node.js](https://nodejs.org/), [Avahi](https://www.avahi.org/), and [Homebridge](https://homebridge.io/):

```bash
# Install Avahi if needed
sudo apt-get install libavahi-compat-libdnssd-dev

# Install Homebridge (official method)
curl -sSfL https://repo.homebridge.io/KEY.gpg | sudo gpg --dearmor | sudo tee /usr/share/keyrings/homebridge.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/homebridge.gpg] https://repo.homebridge.io stable main" | sudo tee /etc/apt/sources.list.d/homebridge.list > /dev/null
sudo apt-get update
sudo apt-get install homebridge

# Install this plugin
sudo npm install -g homebridge-ruuvitag
```

> After installing the plugin on Homebridge 2.x, follow the [Fix for Homebridge 2.x / Node 22](#fix-for-homebridge-2x--node-22-raspberry-pi) section above.

## Find out RuuviTag IDs

You can find your RuuviTag IDs by running [`ruuvitag-debug`](https://github.com/pakastin/ruuvitag-debug):

```bash
npx ruuvitag-debug
```

## Config

Add your tags to `/var/lib/homebridge/config.json` (Homebridge 2.x path) or `~/.homebridge/config.json`:

```json
{
  "bridge": {
    "name": "Homebridge",
    "username": "xxx",
    "port": xxx,
    "pin": "xxx"
  },
  "accessories": [
    {
      "accessory": "Ruuvitag",
      "name": "Living Room",
      "id": "xxx"
    },
    {
      "accessory": "Ruuvitag",
      "name": "Bedroom",
      "id": "xxx"
    },
    {
      "accessory": "Ruuvitag",
      "name": "Balcony",
      "id": "xxx"
    }
  ]
}
```

### Socket option

You can listen to RuuviTag update events emitted from a [socket server](https://github.com/klaalo/ifData/tree/master/tagSocket) instead of using Bluetooth:

```json
"socket": "http://raspberrypi.local:8787"
```

## Supported features
- Temperature
- Humidity
- Battery level
- Battery level alert
- Heat alert
- Cold alert
- High humidity alert
- Low humidity alert
- Motion alert
