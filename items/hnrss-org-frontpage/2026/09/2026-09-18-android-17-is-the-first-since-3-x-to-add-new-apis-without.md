---
title: Android 17 is the first since 3.x to add new APIs without releasing to the AOSP
link: https://grapheneos.social/@GrapheneOS/117282080803799576
source: hnrss-org-frontpage
published: 2026-09-18T19:03:09Z
updated: 2026-09-18T19:03:09Z
first_seen: 2026-09-18T21:49:41.060765696Z
authors:
- theanonymousone
summary: 'Article URL: https://grapheneos.social/@GrapheneOS/117282080803799576 Comments URL: https://news.ycombinator.com/item?id=49758736 Points: 314 # Comments: 143'
content: extracted
html: 2026-09-18-android-17-is-the-first-since-3-x-to-add-new-apis-without.html
---

Android 17 QPR1 is the first release since Android Honeycomb (3.x) adding new APIs for app developers without a release to the Android Open Source Project. The new APIs are currently exclusive to the Pixel OS and aren't available to other Android OEMs.

[https:// developer.android.com/sdk/api\_ diff/37.1/changes](https://developer.android.com/sdk/api_diff/37.1/changes)

Non-Google Android OEMs and AOSP-based projects can ship yearly and QPR2 releases. There are also security backports to those releases. Security preview access is needed to ship patches without months of delay. We've had that since before our Motorola partnership via another OEM.

We already ported our code to Android 17 QPR1 since before it was released on September 15th but don't have permission to release it yet. We're working on backporting Pixel firmware, kernel drivers, userspace drivers and HALs from Android 17 QPR1 to Android 17 for now instead.

Pixel Update Bulletin for September 2026 has additional patches to standard Android platform components used by non-Pixel devices. These patches are relevant to non-Pixel devices but haven't been made available via the September 2026 Android Security Bulletin or preview patches.

Google should not be gatekeeping security patches to the standard Android platform code from Android OEMs but that's what they've started doing. Other OEMs will get these patches in December 2026 via Android 17 QPR2. We can ship them early by reverse engineering the code instead.

It would be interesting to know if Google's legal team is aware they're giving Pixels months of early access to new Android features and bug fixes including certain important security patches. Pixels being given this competitive edge over Google's OEM partners is very dubious.

Google is also once again failing to comply with our GPL source requests for weeks. We requested CD1A.260905.001.A1 sources on September 1st and were only provided access today. The kernel build IDs are the same for 17 QPR1 Beta 9 and 17 QPR1 so we have that already at least.

We could have shipped an Android 17 QPR1 update already since our port was completed. Instead, we have to deal with ongoing pain until Android 17 QPR2 is released in December 2026. This will not be an issue for Motorola since we'll have official firmware and driver code provided.

Pixels are now significantly harder to support than many other devices. One of the only advantages of Pixels is now a disadvantage instead. They're still the best fit for us due to the updates and security features but it burns time we want to spend on privacy and security work.

It will be far easier for us to support upcoming Motorola devices than Pixels. Qualcomm will hopefully expand MTE support beyond the highest end flagship SoC soon so we can expand to more than flagships. Pixel 11 didn't remove it from hardware, only firmware, so that's good news.
