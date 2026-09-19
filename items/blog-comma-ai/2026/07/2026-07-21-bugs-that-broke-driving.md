---
title: Bugs that broke driving
link: https://blog.comma.ai/driving-bugs/
source: blog-comma-ai
published: 2026-07-21T20:00:00Z
updated: 2026-07-21T20:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Harald
summary: All software is riddled with bugs, and usually bad software is bad because of those bugs, not because of bad algorithmic choices. openpilot is no different; we’ve made great progress over the years by implementing new exciting ML techniques, but most of the driving …
content: extracted
html: 2026-07-21-bugs-that-broke-driving.html
preview:
  file: 2026-07-21-bugs-that-broke-driving.preview-2397efb998fc.webp
  width: 256
  height: 144
  alt: Road-camera view with a false laneline through the center of the road.
  color: '#4f5f59'
images:
- source: https://blog.comma.ai/img/bug_stories/laneline_middle_of_road.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-d07981253b4c.png
    width: 1913
    height: 1073
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-7bacb5182ca2.webp
    width: 320
    height: 179
  - file: 2026-07-21-bugs-that-broke-driving.image-34615d162dd1.webp
    width: 640
    height: 359
  - file: 2026-07-21-bugs-that-broke-driving.image-bcd99cc1b8fa.webp
    width: 960
    height: 538
  - file: 2026-07-21-bugs-that-broke-driving.image-cf2d54c4f02c.webp
    width: 1280
    height: 718
  - file: 2026-07-21-bugs-that-broke-driving.image-e86c8ceb5173.webp
    width: 1600
    height: 897
  - file: 2026-07-21-bugs-that-broke-driving.image-e05c4390a2d5.webp
    width: 1913
    height: 1073
  color: '#455747'
- source: https://blog.comma.ai/img/bug_stories/hood_reflection_laneline.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-11b5e04908f8.png
    width: 1764
    height: 877
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-dfe9c8902e5a.webp
    width: 320
    height: 159
  - file: 2026-07-21-bugs-that-broke-driving.image-4f68f88b35dd.webp
    width: 640
    height: 318
  - file: 2026-07-21-bugs-that-broke-driving.image-2e973c1a70de.webp
    width: 960
    height: 477
  - file: 2026-07-21-bugs-that-broke-driving.image-aa0626e65a89.webp
    width: 1280
    height: 636
  - file: 2026-07-21-bugs-that-broke-driving.image-fd618dbaaca8.webp
    width: 1764
    height: 877
  color: '#000000'
- source: https://blog.comma.ai/img/bug_stories/double_yellow_tf_model.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-c2673cb6ab62.png
    width: 1558
    height: 1155
  color: '#0a0618'
- source: https://blog.comma.ai/img/bug_stories/garbage_orb.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-6c725093030b.png
    width: 976
    height: 416
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-5a79d2ce3e38.webp
    width: 320
    height: 136
  - file: 2026-07-21-bugs-that-broke-driving.image-519bdc7fa586.webp
    width: 640
    height: 273
  - file: 2026-07-21-bugs-that-broke-driving.image-01a4450a837e.webp
    width: 960
    height: 409
  - file: 2026-07-21-bugs-that-broke-driving.image-81fef4b81695.webp
    width: 976
    height: 416
  color: '#060106'
- source: https://blog.comma.ai/img/bug_stories/pitch_accel_baseline_report.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-dc3f0248e889.png
    width: 1599
    height: 770
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-a46c845ae0f4.webp
    width: 320
    height: 154
  - file: 2026-07-21-bugs-that-broke-driving.image-c8dffd5a4bbf.webp
    width: 640
    height: 308
  - file: 2026-07-21-bugs-that-broke-driving.image-138e5593b219.webp
    width: 960
    height: 462
  - file: 2026-07-21-bugs-that-broke-driving.image-047864c63721.webp
    width: 1280
    height: 616
  - file: 2026-07-21-bugs-that-broke-driving.image-94df50c6201b.webp
    width: 1599
    height: 770
  color: '#fbfbfb'
- source: https://blog.comma.ai/img/bug_stories/pitch_accel_coupled_report.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-ef052f24a732.png
    width: 1599
    height: 770
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-2a7f7794aa87.webp
    width: 320
    height: 154
  - file: 2026-07-21-bugs-that-broke-driving.image-9cde5422735b.webp
    width: 640
    height: 308
  - file: 2026-07-21-bugs-that-broke-driving.image-d5c369634354.webp
    width: 960
    height: 462
  - file: 2026-07-21-bugs-that-broke-driving.image-c4230c6a7390.webp
    width: 1280
    height: 616
  - file: 2026-07-21-bugs-that-broke-driving.image-359ae3c0d7c8.webp
    width: 1599
    height: 770
  color: '#fbfbfc'
- source: https://blog.comma.ai/img/bug_stories/padding_border_comparison.png
  original:
    file: 2026-07-21-bugs-that-broke-driving.image-1bf0d497a02c.png
    width: 1428
    height: 401
  variants:
  - file: 2026-07-21-bugs-that-broke-driving.image-5020959c1d27.webp
    width: 320
    height: 90
  - file: 2026-07-21-bugs-that-broke-driving.image-1030c25ac486.webp
    width: 640
    height: 180
  - file: 2026-07-21-bugs-that-broke-driving.image-ef455d5dd836.webp
    width: 960
    height: 270
  - file: 2026-07-21-bugs-that-broke-driving.image-0f595e2d2502.webp
    width: 1280
    height: 359
  - file: 2026-07-21-bugs-that-broke-driving.image-41f03c0650a6.webp
    width: 1428
    height: 401
  color: '#fcfcfc'
---

July 21, 2026 11 minute read

All software is riddled with bugs, and usually bad software is bad because of those bugs, not because of bad algorithmic choices. openpilot is no different; we’ve made great progress over the years by implementing new exciting ML techniques, but most of the driving improvements came from discovering and fixing blatant bugs. Here I walk through some of the most entertaining bugs through the years.

## openpilot drifts right [Permalink](https://blog.comma.ai/driving-bugs/#openpilot-drifts-right "Permalink")

In 2017 comma was not a mature company. We had few users, low quality logging, no metrics in training other than a unitless val loss, and no closed-loop simulation tests. As a result we generally had no clue what was going on with openpilot beyond obvious issues we noticed while driving with it. This is clear from these Slack messages showing we were aware of the driving model’s tendency to bias driving slightly to the right for months, with no root cause or satisfactory explanation.

By the end of the year we did figure out the major cause of this bias. We trained models to predict the future driving trajectory, and lanelines. These targets were ground-truthed with a localizer that relied on gyro measurements, as well as other sensors. Gyros are usually biased, the Android stack we used automatically corrected for the bias when it detected the device was at rest. This meant that when openpilot started and the car was stationary the bias was corrected appropriately. However, when the car then started moving, the openpilot device would heat up causing the bias to change, but the device never came to rest again so the bias was never corrected. The bias correlation with temperature is similar for every device, so all data would skew the same direction. A more comprehensive localizer that accounted for bias accurately fixed this.

## Laneline predictions in the middle of the lane [Permalink](https://blog.comma.ai/driving-bugs/#laneline-predictions-in-the-middle-of-the-lane "Permalink")

![Road-camera view with a false laneline through the center of the road.](https://blog.comma.ai/img/bug_stories/laneline_middle_of_road.png)

In 2019 we started getting mysterious complaints of cars departing the lane on roads with clearly marked lanelines. This happened because at the time openpilot still relied on the lanelines for driving decisions, and as the image shows, in these cases it would inexplicably detect a laneline in the middle of the lane. After looking for some more cases like this, we found all examples were in situations with bright daylight.

![Road-camera view with false lanelines drawn over reflections on a car hood.](https://blog.comma.ai/img/bug_stories/hood_reflection_laneline.png)

White reflections at front of hood classified as lanelines

The issue became obvious when looking at several examples of the training data with this same flaw. In certain lighting conditions the sun can reflect on the hood showing a bright white area, this gets detected in the ground-truth stack as a laneline marking that is always just in front of the car, and thus gets reconstructed as a laneline in the middle of the road. By adding better ground-truth of specifically these examples to the laneline detector, we retrained the detector and trained a new driving model with the fix.

## Turn-cutting in highway turns [Permalink](https://blog.comma.ai/driving-bugs/#turn-cutting-in-highway-turns "Permalink")

![Night road scene with incorrect double-yellow lane predictions.](https://blog.comma.ai/img/bug_stories/double_yellow_tf_model.png)

In October 2020 there were several complaints about openpilot crossing the inner laneline in turns. After looking at the examples they all appeared to happen in long sweeping turns at night, in all cases the driving model predictions for the path and lanelines that were biased towards the inside of the turn.

An investigation into the ground-truth showed the localizer incorrectly computed the car’s motion in these cases. Some localizer debugging revealed that the headlights shining onto long continuous lanelines creates problems for the visual odometry. The ORB feature detector selected edges based on the headlight illumination that move vehicle with the vehicle. This means that in scenes where there are little other features for the ORB detector to track, the localizer relies only on these erroneous features causing the localizer to be consistently biased towards those inside of the turn. Smarter ORB feature matching and outlier rejection fixed this, and the retrained model addressed the complaints.

![Night road scene showing tracked ORB points on a double-yellow line.](https://blog.comma.ai/img/bug_stories/garbage_orb.png)

Poor ORB features on a double-yellow line.

## Braking blurs the image [Permalink](https://blog.comma.ai/driving-bugs/#braking-blurs-the-image "Permalink")

Phone cameras use a voice coil actuator to move the lens for focus adjustment. This means the lens is not rigidly mounted to a focus position, but instead mounted on a spring and focus movements are achieved by applying a force with the actuator. This also means that if there are rapid accelerations of the device, focus will shift due to the inertial forces, and to maintain focus a compensatory force needs to be applied with the actuator.

We had an algorithm that would compensate for this focus shift for the OnePlus phones we were using in comma devices in 2019. Later we switched to Leeco phones, but never re-evaluated the focus algorithm. As it turns out our compensation algorithm was causing significant blur during acceleration on these newer devices, and these phones needed no focus compensation at all. This algorithm caused exactly what it was designed to prevent!

With sag compensation (left); without it (right), December 2021.

## Driving model never converges to good highway speeds [Permalink](https://blog.comma.ai/driving-bugs/#driving-model-never-converges-to-good-highway-speeds "Permalink")

When experimental mode was introduced in openpilot, openpilot did not decisively accelerate or brake to a good speeds on the highway. Instead in steady-state, it would just keep driving the speed it was initialized at. Which could for example cause it to drive 55mph forever on an empty long highway.

Hard braking event at −9.5 m/s², notice the slight pitch movements as a result.

openpilot driving models are trained on-policy in a driving simulator. The inputs to the simulator are 6dof pose changes, we use this instead of actions (acceleration and curvature) so we have more fine-grained control over the physics of the simulation. We noticed that we needed to add a lot of pitch noise to the simulation for the model to learn to brake and accelerate.

After investigation this was due to the tight coupling of pitch and acceleration. In any normal vehicle, acceleration causes pitch movements. This means acceleration must be simulated with an appropriate pitch change. This lack of realism in the simulator can be exploited by the student model that is trained in it. As the results below show, fixing these artifacts by simulating realistic pitch movements during simulated acceleration made the model perform much better at our speed convergence tests.

[![Baseline speed-convergence report showing that none of nine runs converged.](https://blog.comma.ai/img/bug_stories/pitch_accel_baseline_report.png)](https://blog.comma.ai/img/bug_stories/pitch_accel_baseline_report.png "Open image at full size")

Baseline: 0 of 9 runs converged

[![Speed-convergence report with realistic pitch-and-acceleration coupling showing two of six runs converged.](https://blog.comma.ai/img/bug_stories/pitch_accel_coupled_report.png)](https://blog.comma.ai/img/bug_stories/pitch_accel_coupled_report.png "Open image at full size")

Realistic coupling: 2 of 6 runs converged

## Runtime performance doesn’t match train-time tests [Permalink](https://blog.comma.ai/driving-bugs/#runtime-performance-doesnt-match-train-time-tests "Permalink")

[![Side-by-side model inputs showing a replicated training border and a green zero-filled runtime border.](https://blog.comma.ai/img/bug_stories/padding_border_comparison.png)](https://blog.comma.ai/img/bug_stories/padding_border_comparison.png "Open image at full size")

This bug is so good we wrote it 4 times. Over the years openpilot has supported various cameras and mounting configurations. As a result sometimes the models were trained to take in bigger images than were available in some configurations. To make this work, the images needed to be padded to fit to the model’s desired input shape. It is essential that the padding is the same during train-time and runtime, but the inference and training stacks are completely separate. Over the years they each changed frequently, and were poorly tested to match each other.

Here are all the varieties of bugs with padding making runtime different from train-time causing regressed outputs:

- in 2021: [`fc19544` (#22971)](https://github.com/commaai/openpilot/commit/fc1954492b860011ca3b02ca69793e1fc9407d26)
- in 2024: [`8656ef1` (#31495)](https://github.com/commaai/openpilot/commit/8656ef12c5e90881c60f730617645a586d922d24)
- in 2026 [`f24ad7e` (#37986)](https://github.com/commaai/openpilot/commit/f24ad7e27aaf90fd7a6dedc56a21648b191dc3fe)
- also in 2026 [`4585e93` (#38091)](https://github.com/commaai/openpilot/commit/4585e93066b86ef8afa0788b7d0dd7249c66932d)

## Looking back [Permalink](https://blog.comma.ai/driving-bugs/#looking-back "Permalink")

A lot has changed since 2017, many of these bugs would be unlikely to make it to production with our current tooling and infrastructure. We will still write many silly bugs though, and if you like the idea of fixing those bugs, [maybe you should come work here.](https://comma.ai/jobs)

*Harald Schäfer*\
 *CTO @ [comma.ai](https://comma.ai)*
