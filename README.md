## Cafy

This is an experimental Node.js app to interact with the Primadonna Elite (ECAM65075MS) coffee smart machines (and probably other ECAM models), using the Bluetooth Low Energy (BLE) protocol.

### Disclaimer

I own a Primadonna Elite (ECAM 650.75.MS) and I created this app for my personal use. **The code shared in this project is for educational purposes only.**

The code is provided “as is”, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and noninfringement. In no event shall I (Wassim Chegham) or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the code or the use or other dealings in the code.

# Prerequisites

You will need:

- [Node v15 and npm](https://nodejs.org/en/download/current/)
- [the git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) command line
- [Code editor and a terminal](https://code.visualstudio.com/?WT.mc_id=javascript-00000-wachegha)
- Make sure your computer has a Bluetooth adapter that supports [BLE](https://en.wikipedia.org/wiki/Bluetooth_Low_Energy).

# Getting Started

1. `git clone https://github.com/manekinekko/node-ecam-coffee.git`
1. `cd node-ecam-coffee`
1. `npm install`
1. `npm start`

## Sending commands (WIP)

For the time being, you can just edit the [index.ts](src/index.ts) file. We will provide a cleaner public API in the future.

```typescript
import { App } from "./classes";

(async function () {
  let app = new App();
  await app.sendCommand(["0d 08 83 f0 02 02 06 c4 b1"]);
})();

> NOTE: you will need to rebuild your code (`npm start`) every time you change a file under [src](src/) folder.

```

## Project Status

This project is still under development. Most of the work is being invested in understanding the communication protocol and decoding the packets coming from the machine (see [src/decoder.ts](src/decoder.ts)).

### Beverages

| Beverage                                   | Trigger Command                                            | Stop Command                 |
| ------------------------------------------ | ---------------------------------------------------------- | ---------------------------- |
| Coffee                                     | `0d 0f 83 f0 02 01 01 00 67 02 02 00 00 06 77 ff`          | `0d 08 83 f0 02 02 06 c4 b1` |
| Doppio+                                    | `0d 0d 83 f0 05 01 01 00 78 00 00 06 c4 7e`                | `0d 08 83 f0 05 02 06 41 21` |
| Steam                                      | `0d 0d 83 f0 11 01 09 03 84 1c 01 06 c0 7b`                | `0d 08 83 f0 11 02 06 de 82` |
| Hot Water                                  | `0d 0d 83 f0 10 01 0f 00 fa 1c 01 06 04 b4`                | `0d 08 83 f0 10 02 06 e9 b2` |
| x2 Espresso                                | `0d 0f 83 f0 04 01 01 00 28 02 02 00 00 06 ab 53`          | `0d 08 83 f0 04 02 06 76 11` |
| Americano                                  | `0d 12 83 f0 06 01 01 00 28 02 03 0f 00 6e 00 00 06 47 8b` | `0d 08 83 f0 06 02 06 18 71` |
| Coffee Long                                | `0d 0f 83 f0 03 01 01 00 a0 02 03 00 00 06 18 7f`          | `0d 08 83 f0 03 02 06 f3 81` |
| Espresso x1 (Aroma=3 Temperature=2 Qty=40) | `0d 11 83 f0 01 01 01 00 28 02 03 08 00 00 00 06 8f fc`    | `0d 08 83 f0 01 02 06 9d e1` |

### Machine settings

| Setting           | Trigger Command                       |
| ----------------- | ------------------------------------- |
| Turn on           | `0d 07 84 0f 02 01 55 12`             |
| Cup Light On      | `0d 0b 90 0f 00 3f 00 00 00 99 39 22` |
| Cup Light Off     | `0d 0b 90 0f 00 3f 00 00 00 91 b8 2a` |
| Cup Warmer On     | `0d 0b 90 0f 00 3f 00 00 00 b1 9c 48` |
| Cup Warmer Off    | `0d 0b 90 0f 00 3f 00 00 00 91 b8 2a` |
| Energy Saving On  | `0d 0b 90 0f 00 3f 00 00 00 91 b8 2a` |
| Energy Saving Off | `0d 0b 90 0f 00 3f 00 00 00 81 aa 1b` |
| Beep Sound On     | `0d 0b 90 0f 00 3f 00 00 00 91 b8 2a` |
| Beep Sound Off    | `0d 0b 90 0f 00 3f 00 00 00 95 f8 ae` |
| Show Time         | `0d 08 95 0f 00 5f 03 00 eb`          |
| Water Hardness 1  | `0d 0b 90 0f 00 32 00 00 00 00 0a c8` |
| Water Hardness 2  | `0d 0b 90 0f 00 32 00 00 00 02 2a 8a` |
| Water Hardness 3  | `0d 0b 90 0f 00 32 00 00 00 02 2a 8a` |
| Water Hardness 4  | `0d 0b 90 0f 00 32 00 00 00 03 3a ab` |

### Other

|                               |                       |
| ----------------------------- | --------------------- |
| Beverage Statistics           | TODO                  |
| Profiles                      | TODO                  |
| "My" Custom Beverage settings | TODO                  |
| Monitor data                  | WIP                   |
| Decoding machine responses    | [WIP](src/decoder.ts) |



# Getting started with Docker

Download [Docker Desktop](https://www.docker.com/products/docker-desktop) for Mac or Windows. [Docker Compose](https://docs.docker.com/compose) will be automatically installed. 
On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/). 

Download Run in this directory:
```
git clone https://github.com/manekinekko/node-ecam-coffee.git
cd node-ecam-coffee
docker-compose up -d
```
The app will be running at [http://localhost:8080](http://localhost:8080), and the results will be at [http://localhost:8080](http://localhost:8080).


# Protocol

## Request/Response Packet Format

### Request Packet

```text
  00              01                      N                n-1                n
+----+----------------------------+---+---+---+---+------------------+----------------+
| 0d |   request packet size (n)  |     data      |  checksum byte   | checksum byte  |
+----+----------------------------+---+---+---+---+------------------+----------------+
```

### Response Packet

```text
  00               01                      N              n-1                 n
+----+----------------------------+---+---+---+---+------------------+----------------+
| d0 |   request packet size (n)  |     data      |  checksum byte   | checksum byte  |
+----+----------------------------+---+---+---+---+------------------+----------------+
```

### Checksum Algorithm

TODO

## Monitoring Data

### Request Packet

```text
  00   01   02   03   04   05
+----+----+----+----+----+----+
| 0d | 05 | 75 | 0f | da | 25 |
+----+----+----+----+----+----+

00= Request magic byte
01= Request packet size
02= Monitoring data type (T0=0x70, T1=0x75, T2=0x75)
03= 0x0f (??)
04= Checksum byte
05= Checksum byte
```

### Response Packet

```text
  00   01   02   03   04   05   06   07   08   09   10   11   12   13   14   15   16   17   18
+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+
| d0 | 12 | 75 | 0f | 01 | 01 | 00 | 08 | 00 | 00 | 02 | 00 | 00 | 00 | 00 | 00 | 00 | 7d | 05 |
+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+----+

00= Response magic byte
01= Response packet size
02= Operation ID
03= ??
04= Accessory Present (00: no accessory, 01: water spout, 02: milk spout, 03: chocolate spout, 04: milk clean dial)
05= ??
06= ??
07= ??
08= ??
09= ??
10= Machine Model Id
11= Dispensing Percentage
12= ??
13= ??
14= ??
15= Main Board Software Release
16= ??
17= Checksum byte
18= Checksum byte
```

## Troubleshooting

#### Running on Linux/RPi (EPERM, Operation not permitted)

If you get the following error:

```text
hci onSocketError: EPERM, Operation not permitted
noble warning: adapter state unauthorized, please run as root or with sudo
               or see README for information on running without root/sudo:
               https://github.com/sandeepmistry/noble#running-on-linux
```

You need to apply the following command:

```bash
sudo setcap cap_net_raw+eip $(eval readlink -f `which node`)
```

More info here: https://github.com/noble/noble#running-on-linux
