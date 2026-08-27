# homebridge-ruuvitag

> **⚠️ Homebridge 2.x compatible fork** — This branch (`homebridge-2`) fixes the `TypeError: Service.BatteryService is not a constructor` crash introduced in Homebridge 2.0. It is the recommended version to use.

With this [Homebridge](https://github.com/homebridge/homebridge) plugin you can use [RuuviTags](https://tag.ruuvi.com/) with [Apple HomeKit](https://www.apple.com/ios/home/).

## What's fixed in this version

- **Homebridge 2.x compatibility** — `Service.BatteryService` and all deprecated `Service.*` / `Characteristic.*` shorthand constructors have been replaced with the new `hap.Service.*` / `hap.Characteristic.*` API required by Homebridge 2.0+.
- Tested on **Homebridge v2.4.0** and **Node.js v22**.

## Installation

### Recommended — install directly from this branch

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

## Find out Ruuvitag IDs

```bash
npx ruuvitag-debug
```

## Config

Add your tags to your Homebridge `config.json` under `accessories`:

```json
{
  "bridge": {
    "name": "Ruuvi",
    "username": "CC:22:3D:E3:CE:30",
    "port": 51826,
    "pin": "031-45-154"
  },
  "accessories": [
    {
      "accessory": "Ruuvitag",
      "name": "Bathroom",
      "id": "ca67bf52ca12"
    },
    {
      "accessory": "Ruuvitag",
      "name": "Bedroom",
      "id": "fa81b4c6a891"
    }
  ]
}
```

### Socket option

You can listen to RuuviTag update events from a [socket server](https://github.com/klaalo/ifData/tree/master/tagSocket) instead of Bluetooth:

```json
"socket": "http://raspberrypi.local:8787"
```

---

## Updates

- **5.3.0** *(this branch)*: Fixed Homebridge 2.x compatibility (`Service.BatteryService` constructor error)
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
