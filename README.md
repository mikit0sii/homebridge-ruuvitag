# homebridge-ruuvitag

With this [Homebridge](https://github.com/nfarina/homebridge) plugin you can use [RuuviTags](https://tag.ruuvi.com/) with [Apple HomeKit](https://www.apple.com/ios/home/).

## Updates
- 1.7.0: Added support for latest Node.js versions!
- 1.5.0: [Humidity triggers!](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.5.0)
- 1.4.0: [Disable temp/humidity](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.4.0)
- 1.3.1: [Enhanced movement formula](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.1)
- 1.3.0: [Motion triggers!](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.3.0)
- 1.2.0: [You can now set up heat and cold triggers](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.2.0)
- 1.1.0: [Show battery level + low battery warning](https://github.com/pakastin/homebridge-ruuvitag/releases/tag/v1.1.0)

## Installation
First, install [Avahi](https://www.avahi.org/) (Homebridge needs this), [Homebridge](https://github.com/nfarina/homebridge) and this plugin
_(you also need [Node.js](https://nodejs.org/) installed)_:
```bash
sudo apt-get install libavahi-compat-libdnssd-dev
sudo npm i -g homebridge
sudo npm i -g homebridge-ruuvitag
```

## Find out Ruuvitag ID's
You can find out Ruuvitag ID's by installing and running [`ruuvitag-debug`](https://github.com/pakastin/ruuvitag-debug):
```bash
sudo npm -g i ruuvitag-debug
ruuvitag-debug
```

## Config

Create a [`~/.homebridge/config.json`](https://github.com/nfarina/homebridge/blob/master/config-sample.json) file
(change ID's and add/remove tags as necessary):

```json
{
  "bridge": {
    "name": "Homebridge",
    "username": "XX:XX:XX:XX:XX:XX",
    "port": 51826,
    "pin": "XXX-XX-XXX"
  },

  "description": "RuuviTag bridge",

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
    },
    {
      "accessory": "Ruuvitag",
      "name": "Balcony",
      "id": "xxxxxxxxxxxx"
    }
  ]
}
```

> 🔑 **Never commit your real `config.json`** — it contains your bridge PIN and MAC address.

## Run

Now you can run Homebridge:
```bash
homebridge
```

## Start on startup

Install pm2:
```bash
npm -g i pm2
```

Start with pm2 and save as daemon:
```
pm2 start homebridge
pm2 save
pm2 startup
```

## Supported features
For now the bridge only supports temperature, humidity, battery level and warning for low battery.
