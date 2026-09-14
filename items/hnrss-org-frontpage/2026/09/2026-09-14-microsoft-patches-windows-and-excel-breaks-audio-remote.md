---
title: Microsoft patches Windows and Excel – breaks audio, remote access, and paste
link: https://www.theregister.com/os-platforms/2026/09/14/microsoft-patches-windows-and-excel-breaks-audio-remote-access-and-paste/5296085
source: hnrss-org-frontpage
published: 2026-09-14T16:09:45Z
updated: 2026-09-14T16:09:45Z
first_seen: 2026-09-14T22:43:23.776526965Z
authors:
- Alephinitesimal
summary: 'Article URL: https://www.theregister.com/os-platforms/2026/09/14/microsoft-patches-windows-and-excel-breaks-audio-remote-access-and-paste/5296085 Comments URL: https://news.ycombinator.com/item?id=49699297 Points: 173 # Comments: 88'
content: extracted
html: 2026-09-14-microsoft-patches-windows-and-excel-breaks-audio-remote.html
preview:
  file: 2026-09-14-microsoft-patches-windows-and-excel-breaks-audio-remote.preview-5d356a7a42eb.webp
  width: 256
  height: 146
  color: '#b3b3b3'
images:
- source: https://image.theregister.com/259053.jpg?imageId=259053&x=0&y=0&cropw=100&croph=100&panox=0&panoy=0&panow=100&panoh=100&width=1200&height=683
  original:
    file: 2026-09-14-microsoft-patches-windows-and-excel-breaks-audio-remote.image-73784bb2c8ba.jpg
    width: 1200
    height: 683
  color: '#b9b9b9'
---

OS Platforms

Redmond's quality drive takes another detour through the known issues list

Microsoft has confirmed that its latest security updates can disrupt Remote Desktop Services, silence some USB audio devices, and break pasting in Excel.

September's Windows patches hardly support Microsoft's insistence that [it is sorting out quality](https://www.theregister.com/software/2026/03/24/windows-boss-promises-to-heal-the-operating-systems-wounds/5223630). The known issues list suggests there's still work to do.

Reports of problems with Remote Desktop Services (RDS) began circulating on social media shortly after the update, and Microsoft has now [acknowledged](https://learn.microsoft.com/en-us/windows/release-health/status-windows-11-25h2#4981msgdesc) that, for some users, RDS has indeed been broken across multiple Windows versions, including Windows 11 26H1 and Windows Server 2012.

REG AD

The latter is due to [drop out of the Extended Security Updates (ESU)](https://learn.microsoft.com/en-us/lifecycle/products/windows-server-2012) program on October 13, 2026, so perhaps administrators might consider this a going-away present from Microsoft?

REG AD

Connections might fail after a few minutes, servers might hang at "Please wait for the Remote Desktop Configuration," and so on.

"Related tools, including Microsoft Management Console (MMC), RDS Licensing Diagnoser, and File Explorer might also become unresponsive," Microsoft admitted.

"Additionally, the Windows Update page might stop responding and continuously display a loading indicator."

If a virtual machine becomes inaccessible through RDP, stopping (deallocating) and restarting it might temporarily restore connectivity. Microsoft is working on a fix.

Microsoft also [confirmed](https://learn.microsoft.com/en-us/windows/release-health/status-windows-11-25h2#4984msgdesc) issues with support for some USB Audio Class 1.0 devices on Windows 11 26H1, 25H2, and 24H2. The standard dates back to the previous century, but affected users might find themselves with no audio, broken sound settings and volume controls, or problems with multichannel audio.

Some customers have restored audio by switching to two-channel mode, Microsoft says. The company is working on a fix but has not provided a timeline.

Users of Microsoft's productivity applications were not left out. either A fix for Excel remote code execution and information disclosure vulnerabilities has broken a basic spreadsheet function.

"The paste operation might fail silently," [according](https://support.microsoft.com/en-gb/servicing/office/hotfix/excel/5002914) to Microsoft.

REG AD

Excel 2016, 2019, 2021, and 2024 are affected. Microsoft said: "Although users try to paste content, the source remains selected and the destination is unmodified. When this issue occurs, users receive no indication of the failure, such as a beep or error message."

The bad news for users with automatic updating turned on is that this update could have already been downloaded and installed automatically. Microsoft has not published a workaround, and one forum user reported resolving the issue by uninstalling and reinstalling Office, while others [reported success](https://learn.microsoft.com/en-us/answers/questions/5999073/is-there-any-fix-to-microsoft-update-kb5002914) using commands to uninstall the security update. Removing the update also removes its security fixes. ®
