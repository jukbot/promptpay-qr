# promptpay-qr 

[![npm version](https://badge.fury.io/js/promptpay-qr.svg)](https://www.npmjs.com/package/promptpay-qr)
[![download](https://img.shields.io/npm/dm/promptpay-qr.svg)](https://www.npmjs.com/package/promptpay-qt)
[![Package Quality](https://npm.packagequality.com/shield/promptpay-qr.svg)](https://packagequality.com/#?package=promptpay-qr)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

`promptpay-qr` project is consists of [Mobile Web App](#mobile-web-app), [Command line](#cli-usage), and [JavaScript library](#api-usage) to help user or developer to generate QR Code payload for PromptPay service.

## Table of contents
   * [Introduction](#introduction)
   * [Projects](#project)
   * [What inside QR code](#what-inside-qr-code)
   * [Mobile Web App](#mobile-web-app)
   * [Installation](#installation)
   * [Usage](#usage)
      * [Command-line](#cli-usage)
      * [Javascript API](#api-usage)
      * [Example](#example)
   * [Implementation in other languages](#implementation-in-other-languages)
   * [Integrations with 3rd party software](#integrations-with-3rd-party-software)
   * [Contribution](#contribution)
   * [References](#references)
   * [License](#license)


## Introduction

The `Bank of Thailand` introduced a [**PromptPay QRCode Standard**](https://thestandard.co/standardqrcode/) that works with most mobile banking apps in Thailand.

By generating a QR code, users of mobile banking apps can scan your QR code, and transfer money to your PromptPay account instantly.

-----------------
## Projects

| Name          | Description   |
| ------------- |-------------  |
| [**promptpay2me**](https://github.com/dtinth/promptpay-qr/tree/master/webapp) | Generate your own QR code wallet to receive money from your mobile home screen. |
| [**promptpay-qr**](#introduction)      | Generate QR code  programmatically to receive money via terminal or using Javascipt API. |

-----------------
### What inside QR code

> ⚠️ Read this section to understand privacy implications of using PromptPay QR code ⚠️

- The PromptPay QR code is based on [EMV QRCPS Merchant Presented Mode](https://www.emvco.com/emv-technologies/qrcodes/) specification.

- **The QR code contains the PromptPay ID.** This means anyone who sees the QR code can find out your PromptPay ID (Phone number, National ID number, e-Wallet ID). Please treat the QR code like your personal information.

- The QR code can optionally contain the transfer amount. Mobile banking apps use this number to pre-fill the amount for convenience.

-----------------
## Mobile Web App

`You can quickly receive money from your mobile home screen.`

<img src="https://cdn.rawgit.com/jukbot/promptpay-qr/09d57279/images/mobile.png" width="320" align="right" />

For example, if you want to collect 80 Baht from each friend, you can open the app, type 80, and show the QR code to your friend.
[It takes less than 5 seconds](https://www.facebook.com/dtinth/videos/10208543817227100/).

To use it:

1. Go to [promptpay2.me](https://promptpay2.me/) on your mobile browser.

2. Tap to set your PromptPay ID.

3. In browser menu select *Add to Home Screen.*

Features:

- The app remembers your PromptPay ID, so you don’t have to enter the information each time.

- The app can keeps your PromptPay ID up to 4 wallets. Which easily manage ID by flipping the card. 

- It is a [**Progressive Web App**](https://developers.google.com/web/progressive-web-apps/), so you can add to home screen and it will work offline (Android).

- No personal data is sent to any server. Your PromptPay ID stays in your phone/computer.

Explore more about this project at [**promptpay2me**](https://github.com/dtinth/promptpay-qr/tree/master/webapp) project section. 

-----------------

## Installation

This library is available as package on [npmjs.com](https://www.npmjs.com/package/promptpay-qr). In order to use this library, you need to have [nodeJS](https://nodejs.org/en/) installed on your local workstation or server. Then run this command to install `promptpay-qr` on your machine.

To install as global package:
```bash
$ npm install -g promptpay-qr
# or
$ yarn add global promptpay-qr
```

To install as dependency in local project:
```bash
$ npm install promptpay-qr
# or
$ yarn add promptpay-qr
```

Want a faster install, try out [Yarn](https://yarnpkg.com/en/)

-----------------

## Usage

You can use this library in several methods including generate via `command-line` or `web browser`.
### CLI Usage

**You can receive money from your terminal.**

Run this command in the terminal to generate QR code:

```bash
$ promptpay-qr 08x-xxx-xxxx
# or
$ promptpay-qr 08x-xxx-xxxx --amount 4.22
# or
$ promptpay-qr 1-xxxx-xxxxx-xx-x --amount 4.22
```

Then QR code will be printed in the terminal.

<p align="center">
  <img src="https://cdn.rawgit.com/jukbot/promptpay-qr/09d57279/images/terminal.png" width="400" />
</p>


### API Usage

You can use this library in your JavaScript app to generate a PromptPay QR code on browser.

### Generate Payload Data

```javascript
const qrcode = require('qrcode')
const generatePayload = require('promptpay-qr')

// Config PromptPay ID
const mobileNumber = '000-000-0000'
const amount = 4.22
const payload = generatePayload(mobileNumber, { amount })

// Display Payload data
console.log(payload)
```

Output
```bash
"00020101021129370016A00000067701011…00660000000005802TH530376463048956"
```

### Generate QR Code

**Browser Side**
```
<!-- index.html -->
<html>
  <body>
    <canvas id="canvas"></canvas>
    <script src="script.js"></script>
  </body>
</html>
```

```javascript
// Import it
const qrcode = require('qrcode')
const generatePayload = require('promptpay-qr')

// Config PromptPay meta data
const mobileNumber = '000-000-0000'
const amount = 4.22
const payload = generatePayload(mobileNumber, { amount })

let target = document.getElementById('canvas')

qrcode.toCanvas(target, payload, function (error) {
  if (error) console.error(error)
})

// Convert to SVG QR Code
QRCode.toCanvas('text',  function (err, canvas) {
  if (err) throw err

  var container = document.getElementById('container')
  container.appendChild(canvas)
})

const options = { type: 'svg', color: { dark: '#003b6a', light: '#f7f8f7' } }
await new Promise((resolve, reject) => {
  qrcode.toString(payload, options, (err, svg) => {
    if (err) return reject(err)
    resolve(svg)
  })
})
```


Returns a string which should be rendered as a QR code.


Output
```bash

```

See more code example in [Runkit](https://runkit.com/dtinth/promptpay-qr).


-----------------

## Implementation in other languages

These libraries are written by other developers:

- PHP: [kittinan/php-promptpay-qr](https://github.com/kittinan/php-promptpay-qr)

- VueJS: [Sellsuki/vue-promptpay-qr](https://github.com/Sellsuki/vue-promptpay-qr)

- Go: [PanJ/promptpay-me](https://github.com/PanJ/promptpay-me)

- Kotlin: [lojorider lojo/medium](https://medium.com/@lojorider/%E0%B8%9E%E0%B8%A3%E0%B9%89%E0%B8%AD%E0%B8%A1%E0%B9%80%E0%B8%9E%E0%B8%A2%E0%B9%8C-%E0%B8%94%E0%B9%89%E0%B8%A7%E0%B8%A2-kotlin-%E0%B9%80%E0%B8%99%E0%B9%89%E0%B8%AD%E0%B9%80%E0%B8%88%E0%B9%89%E0%B8%B2-1c18924d1646)
## Integrations with 3rd party software

These plugins/integrations are written by other developers:

- WordPress: [woodpeckerr/promptpay](https://github.com/woodpeckerr/promptpay) (supports shortcodes and integration with WooCommerce)

-----------------
## Contribution

Pull requests are always welcome. We are always thrilled to receive pull requests and feedback, and do our best to process them as fast as possible. 

If you found any bugs please report issue to 
https://github.com/dtinth/promptpay-qr/issues

## References

- https://www.blognone.com/node/95133
- https://www.emvco.com/emv-technologies/qrcodes/
- https://www.bot.or.th/Thai/PaymentSystems/StandardPS
- https://www.bot.or.th/Thai/PaymentSystems/PSServices/PromptPay
- https://www.bot.or.th/Thai/PaymentSystems/StandardPS/Documents/QRCode.pdf
- https://www.bot.or.th/Thai/PaymentSystems/PSServices/PromptPay/Documents/AboutPromptPay.pdf

## License

[The MIT License](https://github.com/dtinth/promptpay-qr/blob/master/LICENSE)

> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
> IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
> FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
> AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
> OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
> SOFTWARE.
