---
title: Gating entrances with Stripe and NFC passes
link: https://stripe.dev/blog/gating-entrances-with-stripe-and-nfc-passes
source: stripe-com
published: 2025-08-21T00:00:00Z
updated: 2025-08-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Joshua Nussbaum
summary: Learn how to gate physical entrances using Stripe subscriptions and NFC passes. This guide shows how to issue Apple/Google Wallet passes with PassNinja, integrate Stripe Checkout, and control door access with a Raspberry Pi and NFC reader.
content: extracted
html: 2025-08-21-gating-entrances-with-stripe-and-nfc-passes.html
remote_preview:
  url: https://stripe.dev/og/blog/gating-entrances-with-stripe-and-nfc-passes.png
  alt: 'Stripe Developer Blog: Gating entrances with Stripe and NFC passes'
extra:
  content:interactive: true
---

![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image6.png) While working remotely and traveling from place to place, I noticed a recurring pattern.

Every time you arrive in a new city, you rent an Airbnb or check-in to a hotel. Then you sign up for a gym and a co-working space.

Payments for these services are all done electronically. But when it comes to access, you need a plastic card, a fob, or a physical key.

This creates friction for everyone involved. You have to go pick it up, carry it around, hope you don’t lose it, and return it before leaving.

From the business side, they need to pay someone to work the front desk to issue, track, and collect these access devices.

This made me wonder, what if access was fully digital?

## Digital wallet passes

Luckily, there is a better way that is gaining popularity.

Instead of cards and keys, we can issue digital passes with Google Wallet and Apple Wallet and use an NFC reader to gate the entrance.

These digital passes are like certificates that are scannable with NFC. They can be issued and revoked electronically without human intervention (no front desk needed).

Since everyone already has a smart phone or watch with Apple Wallet & Google Wallet, there’s nothing extra to install or carry.

## Integration

Both Apple and Google have public Wallet APIs for issuing passes, but for smaller companies it's easier to use an intermediary like [PassNinja](https://www.passninja.com/).

[PassNinja](https://www.passninja.com/) acts as an abstraction layer on top of Apple & Google, so you only have to integrate one API.

Then you can place an Apple/Google compatible NFC reader near the doorway. We'll use the [DotOrigin](https://www.dotorigin.com/) [VTAP100](https://shop.vtapnfc.com/product/vtap100-embedded-nfc-reader-board/).

## Building it

As an example, we'll build a simple gym membership system.

Members can purchase a Stripe Subscription and a digital pass will be issued to them. The member can then use their phone (or watch) to access the doorway.

![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image5.png)

There will be two codebases:

1. **Website**: A public facing website that accepts payments via Stripe Checkout and issues the digital passes.
2. **Gate**: A private access control system that runs on Linux near the entrance. It will verify the status of the membership by contacting the Stripe Subscription API, and unlock the door for active members.

## Architecture

![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image1.png)

## Hardware

Here’s what you’ll need to build the solution:

### Requirements

- **Computer**: A [Raspberry PI Zero 2W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) or equivalent. For communicating between NFC Reader and Stripe API over WiFi.
- **NFC Reader:** [VTAP100](https://shop.vtapnfc.com/product/vtap100-embedded-nfc-reader-board), an Apple / Google Wallet compatible NFC reader.
- **Door Strike/Bolt:** To unlock and lock the door.
- **Relay:** A [3.3V Relay](https://abra-electronics.com/electromechanical/relays/relay-modules-shields/rm-1-3-3v-t-single-isolated-3-3v-relay-high-low-trigger-module-10a.html) or equivalent. To control locking and unlocking the door strike from the Raspberry PI.
- **PassNinja Account**: To handle issuing of Google & Apple Wallet Passes.

### Wiring diagram

The wiring will look like this:\
 ![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image4.png)

## PassNinja setup

To set up PassNinja:

1. Create an account.

   1. Visit [https://www.passninja.com](https://www.passninja.com)
   2. Click “Get Started”
2. Create a pass template

   1. Log in to PassNinja: [https://www.passninja.com/login](https://www.passninja.com/login)
   2. Click on “Dashboard” in the upper right corner
   3. Click on “New Pass Template” button and create a template for Google & Apple
3. Setup the NFC Reader:

   Follow these instructions [https://www.passninja.com/tutorials/hardware/how-to-configure-a-dot-origin-vtap100-nfc-reader](https://www.passninja.com/tutorials/hardware/how-to-configure-a-dot-origin-vtap100-nfc-reader)

## Purchase flow

The purchase part is on a public website that handles the checkout and issuing of passes.

![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image3.png) We’ll have an endpoint /subscribe to create the Stripe Checkout session and redirect the user to pay:

```
// in web/src/index.ts

// create a Stripe API client
const stripe = new Stripe('<your stripe key>')

// when user visits /subscribe, a Stripe checkout session is created and they're redirected to pay
app.post('/subscribe', async (c) => {
  // this is where the user is redirected after payment`  
  const success_url = new URL('/success?session_id={CHECKOUT_SESSION_ID}', '<your domain>').toString()

  // create a checkout session`  
  const session = await stripe.checkout.sessions.create({
    mode: 'subscription',
    success_url, 
    line_items: [
      { price: '<your price id>', quantity: 1 }
    ]
  })

  // redirect the user to pay
  return c.redirect(session.url)
})
```

When the user finishes paying, Stripe Checkout will redirect them to /success, and the digital pass can then be issued:

```
// in web/src/index.ts

// create the Pass Ninja API client
const passNinja = new PassNinjaClient('<account id>', '<api key>')

// when user visits /success (after checkout completes), issue the digital pass 
app.get('/success', async (c) => {
  // get checkout session_id`  
  const session_id = c.req.query('session_id')

  // get the Stripe checkout session
  const session = await stripe.checkout.sessions.retrieve(session_id)

  // get the Stripe subscription
  const subscription = await stripe.subscriptions.retrieve(session.subscription)

  // ensure the subscription status is active`  
  if (subscription?.status !== 'active') throw new Error('Subscription was not successful')

  // we've confirmed it's paid, so issue a new pass`  
  const pass = await passNinja.pass.create(
    PASSNINJA_PASS_TYPE,
    {
      name: session.customer_details.name,
      email: session.customer_details.email,

       // *important*: save the subscription_id inside the pass
      // this is the value the NFC reader sends during a scan
      "nfc-message": session.subscription
    }  
  )

  // redirect the user to add the pass to their wallet
  return c.redirect(pass.url)
})
```

## Door access control

The door access logic can run on any computer, but we'll use a Raspberry PI Zero 2W which is an inexpensive option (~$15 USD) and has the ability to control a relay.

The sequence looks like this:\
 ![](https://stripe.dev/images/gating-entrances-with-stripe-and-nfc-passes/image2.png)

The NFC reader acts as a virtual serial port, and each time a user’s phone or watch is placed near it, a new line is sent over the serial port.

Linux typically maps virtual serial ports to /dev/ttyACM0. To access it from Node.js, we'll use the npm package [serialport](https://www.npmjs.com/package/serialport).

```
// in gate/src/index.ts`  
import { SerialPort } from ‘serialport’`

// create a serial port client`  
const port = new SerialPort({ path: '/dev/ttyACM0', baudRate: 9600 })
// use ReadlineParser, so that we receive a full lines
const reader = port.pipe(new ReadlineParser({ delimiter: '\r\n' }))

// a new line is sent whenever a pass is near the NFC reader
reader.on('data', async (data) => {`  
  // verify pass here
})
```

The data sent comes from the nfc-message field of the pass, which in our case is the Stripe Subscription ID (starts with sub\_).

We can use that ID to verify that the subscription status is `active`:

```
// in gate/src/index.ts

reader.on('data', async (subscription_id) => {
  // retrieve the subscription record
  const subscription = await stripe.subscriptions.retrieve(subscription_id)

  // check if subscription is active  
  if (subscription?.status === 'active') { 
    // flash LEDs green, play sound, and trigger relay to unlock the door
    success(port)
    console.log(`Access allowed. id=${subscription_id}`)
  } else {
    // flash LEDs red and play sound
    error(port)
    console.error(`Access denied. id=${subscription_id}, status=${subscription?.status}`)
  } 
})
```

The success logic will then open the door bolt by triggering the relay:

```
// in gate/src/index.ts

// setup a connection to the relay on GPIO #8
const relay = new Gpio(8)

// open the door bolt for 5 seconds`  
function success(port) {
  // turn relay on to unlock the door bolt
  relay.high()

  // in 5 seconds, turn relay off. 
  // this causes the door bolt to lock. 
  setTimeout(() => relay.low(), 5_000)
}
```

## Conclusion

Using digital passes makes physical gating much simpler.

It allows merchants to sell and grant access completely digitally, and users don't have to deal with picking up, carrying, replacing, sharing and returning cards and keys.

It’s as easy as issuing a pass after payment, and adding an NFC reader to the entrance to verify the status of payment.

Special thanks to [Bill Scott](https://www.linkedin.com/in/corumba) at [DotOrigin](https://www.dotorigin.com/) for sharing his knowledge on this topic. For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/stripedevelopers).
