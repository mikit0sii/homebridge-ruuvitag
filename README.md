# homebridge-ruuvitag

> **⚠️ Homebridge 2.x compatible fork** — This branch fixes the `TypeError: Service.BatteryService is not a constructor` crash introduced in Homebridge 2.0. It is the recommended version to use.

With this [Homebridge](https://github.com/homebridge/homebridge) plugin you can use [RuuviTags](https://tag.ruuvi.com/) with [Apple HomeKit](https://www.apple.com/ios/home/).

## What's fixed in this version

- **Homebridge 2.x compatibility** — `Service.BatteryService` and all deprecated `Service.*` / `Characteristic.*` shorthand constructors have been replaced with the new `hap.Service.*` / `hap.Characteristic.*` API required by Homebridge 2.0+.
- Tested on **Homebridge v2.4.0** and **Node.js v22**.

## Installation

### Recommended — install directly from this repo

```bash
sudo npm install -g git+https://github.com/mikit0sii/homebridge-ruuvitag.git
```

### If you are using Homebridge UI (hb-service)

1. Open the Homebridge UI in your browser.
2. Go to **Plugins** → search for `homebridge-ruuvitag` → **Uninstall** the current version.
3. Open a terminal on your Homebridge host and run:

```bash
sudo npm install -g git+https://github.com/mikit0sii/homebridge-ruuvitag.git
```

4. Restart Homebridge.

### Legacy — npm (original, Homebridge 1.x only)

```bash
sudo npm i -g homebridge-ruuvitag
```

> ⚠️ The npm registry version (`5.2.0`) is **not compatible** with Homebridge 2.x.

---

## Find out your RuuviTag IDs

Run the debug tool while your tags are broadcasting — it will print each tag's MAC-based ID:

```bash
npx ruuvitag-debug
```

## Config

Add your tags to your Homebridge `config.json` under `accessories`. Replace the placeholder values with your own bridge details and tag IDs:

```json
{
  "bridge": {
    "name": "Homebridge",
    "username": "XX:XX:XX:XX:XX:XX",
    "port": 51826,
    "pin": "XXX-XX-XXX"
  },
  "accessories": [
    {
      "accessory": "Ruuvitag",
      "name": "Living Room",
      "id": "xxxxxxxxxxxx"
    },
    {
      "accessory": "Ruuvitag",
      "name": "Bedroom",
      "id": "xxxxxxxxxxxx"
    }
  ]
}
```

> 🔑 **Never commit your real `config.json`** — it contains your bridge PIN and MAC address. Use this template only as a reference.

### Socket option

You can listen to RuuviTag update events from a [socket server](https://github.com/klaalo/ifData/tree/master/tagSocket) instead of Bluetooth:

```json
"socket": "http://your-server.local:8787"
```

---

## Updates

- **5.3.0** *(this version)*: Fixed Homebridge 2.x compatibility (`Service.BatteryService` constructor error)
- 5.2.0: Added ruuvitag version 5 support (Homebridge 1.x only)
- 2.3.0: Updated ruuvitag support
- 1.8.0: Fixed flooding issue and added `frequency` parameter
- 1.7.0: Added support for latest Node.js versions
- 1.5.0: [Humidity triggers](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.5.0)
- 1.4.0: [Disable temp/humidity](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.4.0)
- 1.3.1: [Enhanced movement formula](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.1)
- 1.3.0: [Motion triggers](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.0)
- 1.2.0: [Heat and cold triggers](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.2.0)
- 1.1.0: [Battery level + low battery warning](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.1.0)

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
