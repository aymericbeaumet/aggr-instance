---
title: Researchers track whole body movement with just a smartwatch
link: https://news.cornell.edu/stories/2026/11/researchers-track-whole-body-movement-just-smartwatch
source: hnrss-org
published: 2026-11-13T17:29:49Z
updated: 2026-11-13T17:29:49Z
first_seen: 2026-11-16T17:21:52.095799575Z
authors:
- PaulHoule
content: extracted
html: 2026-11-13-researchers-track-whole-body-movement-with-just-a-smartwatch.html
remote_preview:
  url: https://news.cornell.edu/sites/default/files/styles/story_thumbnail_lg/public/2026-11/1111_echomotion_0744.jpg?itok=Sru51mwu
  alt: Saif Mahmud, a Ph.D. candidate in the field of information science, demonstrates EchoMotion, a system for tracking body movements using an off-the-shelf smartwatch.
---

For the first time, researchers can track the movement of a person’s entire body with ultrasonic sound waves, using just an off-the-shelf smartwatch and a lightweight artificial intelligence model – all while ensuring privacy and efficiency.

The new technology, called EchoMotion, was developed by Cornell’s  [Smart Computer Interfaces for Future Interactions (SciFi) Lab](https://www.scifilab.org/), directed by [Cheng Zhang](https://bowers.cornell.edu/people/cheng-zhang-0), associate professor of information science in the Cornell Ann S. Bowers College of Computing and Information Science. It uses the built-in speaker and microphone on a commercial smartwatch to determine a person’s body position using a sonar-like sensing system. Once EchoMotion is integrated into commercial wearables or virtual reality (VR) systems, it could be used for tracking workouts, daily activities and progress on physical rehabilitation.

EchoMotion represents a scalable approach that can use AI to understand human behaviors in the real world, Zheng said.

He presented the paper, “[EchoMotion: Towards Full-Body Pose Tracking with a Single Smartwatch Using Active Acoustic Sensing](https://dl.acm.org/doi/epdf/10.1145/3832001),” at the Association for Computing Machinery international joint conference on Pervasive and Ubiquitous Computing and International Symposium on Wearable Computing (UbiComp/IWSC), Oct. 13 in Shanghai.

Saif Mahmud, a Ph.D. candidate in the field of information science, demonstrates EchoMotion, a system for tracking body movements using an off-the-shelf smartwatch.

“This is a very important jump for body tracking,” said Saif Mahmud, doctoral student in the field of information science and lead author of the new research. Existing wearable devices use sensors such as accelerometers and gyroscopes that determine the movement of the device alone, he said. As a result, whole body tracking traditionally has required more complicated setups that may also include smartphones, earbuds or VR headsets.

Some tracking systems also integrate cameras, but EchoMotion’s use of sound waves offers multiple advantages. Acoustic signals preserve privacy for the wearer and the people around them – no need to set up a camera at the gym – and generate smaller amounts of data for easier and cheaper processing.

“While large language models, like ChatGPT, have been in the news, EchoMotion shows how very small models running on wearables have also revolutionized sensing in human-computer interaction,” said co-author  [François Guimbretière](https://infosci.cornell.edu/people/francois-guimbretiere), professor of information science in Cornell Bowers.

The technology also saves on battery power and works with existing off-the-shelf watches. In the prototype, sensing drew about 12 milliwatts, roughly 40 times less than a wearable camera, and a 30-minute session on a Galaxy Watch 6 used only 5% to 6% of the battery.

“We have made significant efforts in making EchoMotion work on commercial devices, so that the adoption can be very easy,” Mahmud said.

To develop the technology, the researchers initially created their own 3D-printed watch replica, which included a microphone and speaker pair on each side. The two speakers emit ultrasonic signals that surround a person in a sphere of inaudible sound waves. Then the two microphones pick up the echoes that bounce off the person’s limbs as they move.

The researchers collected echo patterns from 10 volunteers who wore the watch on their nondominant hand as they performed 18 exercises. Using that data, the researchers developed a deep learning model to translate those patterns into body movements. They measured accuracy by comparing the predicted positions of 12 joints, from head to ankles, with the body poses captured by cameras.

Off-the-shelf smartwatches have only a single microphone and speaker pair, however, so they tested the EchoMotion system on an unmodified Samsung Galaxy Watch 6 with 15 new volunteers. When used by people new to the system, the average error for predicting the location of each joint was under four inches. A one-time, 12-minute calibration cut that error by more than one third, to about 2.4 inches.

The same AI model, without any retraining, also worked on a Samsung Galaxy Watch 7 and a Google Pixel Watch 3.

To see how EchoMotion performed beyond the lab, the researchers asked 10 new participants to wear a smartwatch as they did the 18 exercises in a small room, in a narrow hallway and next to a noisy road. Furniture and ambient noise did not interfere, but the system’s performance suffered slightly in the hallway, due to echoes coming off of other people. Mahmud expects, however, that with more data – from thousands of users instead of dozens – their model will get better at interpreting a wearer’s movements even in crowded environments, like the subway.

Currently, the team is developing the technology for use in fitness tracking, so a personal fitness agent will be able to count a user’s reps and even correct their form, for example, in a Bulgarian split squat. They are also exploring the use of EchoMotion in combination with first-person vision on smartglasses to efficiently track the objects that a person handles.

“In the very near future, with wearables like smartwatches or smartglasses, a personal agent will write a description of your whole day – what you did, what you ate,” Mahmud said. “This project enables the body tracking part of that vision.”

Co-authors on the study include Jiwan Kim of KAIST; Tianhong Catherine Yu, a doctoral student in the field of information science; Kenny Liang ’24 and Vineet Parikh ’20, M.S. ’24.

The research was supported by the National Science Foundation.

*Patricia Waldron is a writer for the Cornell Ann S. Bowers College of Computing and Information Science.*
