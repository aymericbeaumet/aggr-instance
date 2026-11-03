---
title: Stringman, an $800 open-source room scale gripper robot
link: https://neufangled.com/
source: hnrss-org
published: 2026-11-03T16:47:29Z
updated: 2026-11-03T16:47:29Z
first_seen: 2026-11-03T20:48:58.593592327Z
authors:
- nharada
content: extracted
html: 2026-11-03-stringman-an-800-open-source-room-scale-gripper-robot.html
remote_preview:
  url: https://storage.googleapis.com/nf-site-assets-prod-b/assets/install_step1.webp
  alt: Driving a wood screw through a cleat into a wall stud near the ceiling
---

Room-scale cable robot

- ≥ 0.5 kg
- ≥ 1.0 kg
- ≥ 1.5 kg
- ≥ 2.0 kg

Payload capacity at the 28 N line tension limit, net of the 638 g gripper, in a 7 × 7 × 3 m room, anchors in the ceiling corners. [Explore your own room →](https://neufangled.com/payload)

## Tension is all you need

In robotics, price is basically actuator count. Stringman reaches an entire room with four motors, hanging its gripper from lines anchored in the ceiling corners. Working from above, it reaches floors too cluttered for any wheeled or legged robot. And since the lifting force is shared by all motors, payload capacity scales with their combined torque instead of a single actuator's, peaking near the center of the room where the lines share the load most evenly.

| Name | Actuator count | Average payload over work volume |
| ---- | -------------- | -------------------------------- |
| 4    | 1.9 kg         |                                  |
| 7    | 0.32 kg        |                                  |

\*0.75 m serial arm using the same DM-H6215 1 N·m direct drive actuators at rated torque, ignoring the weight of its own links. Stringman figure is for a 7 × 7 × 3 m room at the 28 N line tension limit, net of its 638 g gripper and marker.

## Technical Specs

| Work area                                                                              |
| -------------------------------------------------------------------------------------- |
| Up to 2.2 kg near the floor, 1.9 kg on average.                                        |
| \~0.4 m/s                                                                              |
| 2 anchor cameras @ 1920x1080 30fps and 1 wide FOV gripper camera @ 684x384 60fps       |
| External. (Your PC as controller) Raspberry Pi Zero 2W (gripper) and Pi 3A+ (anchors). |
| 5 GHz Wi-Fi using QR-codes for simple setup. (gripper 2.4 Ghz only)                    |
| 3D printed. Structural parts: PET-GF, Decorative parts: PLA                            |
| Wooden stud backed drywall or plaster wall mounts. Strapping to structural columns.    |
| \~1000h of testing in real homes accumulated so far.                                   |

![Exploded view of the Stringman gripper](https://storage.googleapis.com/nf-site-assets-prod-b/assets/exploded.webp)

## The Courier for Your Home Lab

Give your home lab the capability of carrying parts from one workcell to another anywhere in the room, so you can get more out of your other robots. The [Python API](https://neufangled.com/docs/python_api/) and MCP server is made for agents so you can get your workflow running right away.

[2022 Porsche 911 GT3 RSR](https://sketchfab.com/3d-models/2022-porche-911-gt3-rsr-6370209fd29e4752b53fea3dcceeea13) model by [No Limits](https://sketchfab.com/nolimitsofficial), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/), simplified. SO-101 arm from [TheRobotStudio/SO-ARM100](https://github.com/TheRobotStudio/SO-ARM100), Apache 2.0.

## Unmatched Reach

Unlike desktop arms limited to a fixed radius, Stringman leverages a cable-driven architecture to access the full volume of a room. The work area volume per dollar of this type of robot is completely unmatched, all with fewer actuators than even the simplest mobile robots. Anchors and eylets are mounted in the corners of the room near the ceiling and define a four-sided work area within which any object can be moved from point to point.

|                           | Stringman                   | Research arm |
| ------------------------- | --------------------------- | ------------ |
| 7 × 7 × 3 m room (147 m³) | 0.75 m hemisphere (0.88 m³) |              |
| $795                      | $2,500                      |              |
| 185 m³                    | 0.35 m³                     |              |

That's roughly **500×** the work area volume per dollar, at comparable 2 kg payload.

## What do I need to install Stringman?

- A room where you can secure the anchors to studs or columns.
- A computer to run the controller with some NPU or GPU (macs work great!)
- A handheld drill
- A step ladder that lets you reach the ceiling.

![Driving a wood screw through a cleat into a wall stud near the ceiling](https://storage.googleapis.com/nf-site-assets-prod-b/assets/install_step1.webp)

Secure the cleats to wall studs near the ceiling with wood screws, or to columns with ratchet straps.

![A Stringman anchor hung in the corner of a room below the crown moulding, with lines attached](https://storage.googleapis.com/nf-site-assets-prod-b/assets/install_step2.webp)

Hang the anchors and connect the lines to the gripper.

![Calibration cards laid out on the floor](https://storage.googleapis.com/nf-site-assets-prod-b/assets/install_step3.webp)

Run calibration. This hands off process takes about five minutes and adapts Stringman to your room.

Step-by-step: [picture guide](https://neufangled.com/docs/arp_install_guide/) or [video guide](https://www.youtube.com/watch?v=xKR68cfg3PQ)

### Have a ceiling fan? That's fine.

The original prototype was built in a room with a ceiling fan, and it still runs there today. Mount the anchors at or below the level of the blades and the lines can't physically reach it.

## Training new models

Several methods are available for control with default models for automatic grasping, target, and destination selection to make it easy to quickly build new behaviors.

Stringman is fully capable of [LeRobot](https://huggingface.co/docs/lerobot/en/index) control using including Pi.05, JEPA, Multitask DiT, and others.

Collecting training data for new behaviors can be done from the UI, and as few as 50 episodes can be enough for certain models like DiT to learn a behavior.

## Frequently Asked Questions

### Will there be lines crossing my room?

As long as it remains installed, yes. However, when parked, they are up near the ceiling and out of the way. Clutter on the ceiling is preferable to clutter on the floor in our opinion. If you need to remove the anchors, they can be detached from their mounts without tools, leaving a relatively small white cleat on the wall.

### Can it be used in rooms with ceiling fans?

Yes, it's just a matter of mounting the anchors at or below the level of the fan, making it physically impossible for the lines or marker tag to touch it. Similarly, the work area can be shrunk to exclude low hanging light fixtures if they are off to the side, but if the center of the room does contain low hanging objects, Stringman cannot be installed in that room.

### What happens if someone is caught in the wires?

When reaching a tension limit of 28 newtons (about the weight of a backpack), all motors relax and the lines can be pulled loose. Since they are overhead most of the time, it's uncommon for people to collide with them.

### Can it be controlled remotely?

Not by default, but if you bind it with [neufangled.com/playroom](https://neufangled.com/playroom) then you or anyone you share access with can operate it from anywhere, and access can be revoked at any time. Neufangled Robotics does not store any of the information from your robot that is forwarded through its servers.

![Privacy](https://storage.googleapis.com/nf-site-assets-prod-b/assets/control_panel_light.webp)

## Remote Management

For those who prefer to make the trade-off by binding their robot to an account, our [web-based console](https://neufangled.com/control_panel) allows you to monitor and control Stringman from anywhere. Think of it as a security camera with a hand.

If you do decide that you would like to make use of our cloud telemetry relay, You can use it, and still go back to a completely lan only mode whenever you want. Your data is never collected.

## Set Your Drop Bins

Organizing your home is a physical process, not a coding task. Use our clip-on fiducial tags to designate your laundry bin, toy box, and trash can. Stringman maps these locations visually. You don't need to tag individual pieces of clutter—just the destination. If there's something you never want it to pick up, you can mark that item as off-limits.

Networks trained to control stringman have learned that the word "toybox" refers to that tag, so you can use it to mark anything even if it doesn't look like the toyboxes from our training set.

You can also use the tags as arbitrary source and destination markers for ferry jobs, such as carrying items from one robot arm to another. or carrying laundry from the ground to a robotic folding station

![Stringman installed in a classroom](https://storage.googleapis.com/nf-site-assets-prod-b/assets/stringman-79west-room.webp)

## For Educators

A classroom with a Stringman robot gives your students a chance to become familiar with robotics fundamentals such as inverse kinematics, telemetry, and imitation learning while controlling an exotic and capable flying gripper. If you have walls and a MacBook, you're more than halfway there.

Cross embodiment is one of the biggest challenges in robotics, and showing that a model can operate a vastly different body type is a stronger result than moving it from one bimanual robot to another. Our [datasets are public on Hugging Face](https://huggingface.co/naavox), there's a controller-in-the-loop MuJoCo sim, and Stringman works with LeRobot, making it one of the most inexpensive robots to test on and a natural capstone project.

## No Charging Downtime

Mobile robots spend a significant portion of their time charging. Because Stringman is a powered from the wall, it's always ready for work. Furthermore, by removing the heavy battery pack, we've increased the payload capacity per dollar compared to mobile platforms that must waste energy carrying their own power supply.

The gripper's power is delivered via one of the support cables. A two-conductor FEP jacketed twisted pair cable with a tensile strength of 50N.

## Swing Cancellation

What's that pole for? Stringman's gripper hangs below the pivot point so it can reach the floor next to beds and tables. This additional linkage could allow unconstrained motion so Stringman employs active swing cancellation algorithms to counteract momentum, without adding the cost of any additional motors. This ensures the gripper remains steady and predictable even during rapid cross-room traversals.

There's a whole [video about swing cancellation](https://www.youtube.com/watch?v=d7SvwrUoPOM) if you're curious about how it works!

## Wrist and Fingers

Often, a simple solution that gets the job done has more value than a complex one. Stringman uses a two fingered gripper with a wrist. This is enough dexterity for the clutter it is designed to pick up, keeping weight and cost low and reliability high. In our household tests we have determined that more layers of grip tape is simply a better return on investment than more articulated fingers.

The gripper can rotate it's wrist to better place the fingers around flat sided or long objects.

![A white Stringman anchor in the corner of a room below the crown moulding](https://storage.googleapis.com/nf-site-assets-prod-b/assets/install_step2.webp)

## Preview It on Your Wall

For a easy preview of how it looks in your room, print the anchor mockup in white PLA and hold it up in a corner. [Get the files](https://neufangled.com/docs/downloads/).

## Customize Your Look

TThe Stringman faceplate is easily swappable. Grab the [STL or bambu 3MF files](https://neufangled.com/docs/downloads/) to print your faceplate in any color.
