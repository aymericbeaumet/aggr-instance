---
title: Garage Door Spam
link: https://blog.harrison.page/garage-door-spam/
source: lobste-rs
published: 2026-11-20T02:41:39Z
updated: 2026-11-20T02:41:39Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- blog.harrison.page via zk
labels:
- hardware
summary: Comments
content: extracted
html: 2026-11-20-garage-door-spam.html
remote_preview:
  url: https://harrison.page/harrison.jpg
---

*16 Nov, 2026*

In the beginning, my garage door opener worked fine. When the door opened or closed, I would get a notification. If I'm away from home and the door opens, something weird is going down. The "myQ" app generally worked although it was encrusted with ads:

![Screenshot 2026-10-16 at 14](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/25-1.webp)

I continued using it because I trusted that notifications were meaningful...

...until recently (October 2026):

![Screenshot 2026-10-16 at 13](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/45-1.webp)

This is un-fucking-acceptable. As in, absolutely fucking not.

The manufacturer (Chamberlain) cut off third-party access to its APIs (via myQ) in late 2023. In response, I purchased a pair of [ratgdo32](https://ratcloud.llc/products/ratgdo32) devices to opt out of the bullshit.

> ratgdo is a WiFi control board for your garage door opener that works over your local network using ESP Home or HomeKit

The device is a ~$62 board from ratcloud llc running open-source firmware that wires to the opener's existing terminals and speaks LiftMaster's Security+ 2.0 protocol locally.

![order](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/order.webp)

These were easy enough to hook up. I won't reproduce the instructions here.

The bundle included six black wires, an auxiliary harness I didn’t need.

![bundle](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/bundle.webp)

My router hands each board a fixed address at 192.168.0.192 and 192.168.0.245. Apple's HomeKit just worked and I'm able to control them with voice commands: “Hey Siri, open Door 1”

![homekit](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/homekit.webp)

When I was growing up, my father wired a mercury switch onto our garage door that lit up a light in the hallway when we'd left the garage door open. This was a valuable signal that helped our family avoid accidentally leaving the door open overnight. Since the ratgdo32 has a JSON status endpoint, I was able to rebuild Dad's solution in software with Slack:

![Screenshot 2026-11-01 at 12](https://bear-images.sfo2.cdn.digitaloceanspaces.com/harrisonpage/17-1.webp)

The script runs at night checking for an open door:

```
*/10 21-23,0-5 * * *  garage-watch
```

The repo maintainer made my day when I submitted a [tiny pull request](https://github.com/ratgdo/homekit-ratgdo32/pull/233): "Simple and useful. Happy to merge this. Thank you."

Both boards are now reachable over my Tailscale network, so my monitoring software can load /healthcheck. This required a custom route:

```
/Applications/Tailscale.app/Contents/MacOS/Tailscale set \
  --advertise-routes=192.168.0.192/32,192.168.0.245/32
```

After approving the route, my monitoring software can verify the hardware is working.

Everything runs locally with no cloud, and notifications now say who opened the door which myQ never did.

To anyone at Chamberlain responsible for myQ, I offer my sincerest Fuck You.
