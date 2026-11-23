---
title: GL.iNet Security Notice 2026-11-20
link: https://www.gl-inet.com/en-us/blogs/blog/glinet-security-notice-2026-11-20
source: hnrss-org
published: 2026-11-22T23:06:37Z
updated: 2026-11-22T23:06:37Z
first_seen: 2026-11-23T09:43:22.615423405Z
authors:
- gregsadetsky
content: extracted
html: 2026-11-22-gl-inet-security-notice-2026-11-20.html
remote_preview:
  url: https://www.gl-inet.com/cdn/shop/articles/20261121-145609_1200x675.jpg?v=1795244669
  alt: GL.iNet Security Notice 2026-11-20
---

## About the incident

On November 20, 2026, GL.iNet identified a vulnerability in its cloud remote management services. An unauthorized party exploited the vulnerability to access and install malicious software on some GL.iNet routers and KVM devices.

**The attack path has been blocked.** We fixed the cloud vulnerability and blocked the access used in this incident. Devices can no longer be newly infected through this attack path. The cloud-side fix alone does not remove malware from devices that were already infected; install the security firmware below to remove it.

## Download and install the security update

Install the latest stable security firmware for your exact model as soon as possible, even if the checks below find no signs of infection. **The security firmware update removes the malware identified in this incident without requiring a factory reset. You can keep your existing settings during the update.**

### Router security firmware downloads

- [GL-MG1300](https://dl.gl-inet.com/router/mg1300/stable), [GL-MT3000](https://dl.gl-inet.com/router/mt3000/stable), [GL-MT6000](https://dl.gl-inet.com/router/mt6000/stable) (4.11.1 or later)
- [GL-E5800](https://dl.gl-inet.com/router/e5800/stable) (4.10.3 or later)
- [GL-BE3600](https://dl.gl-inet.com/router/be3600/stable), [GL-BE9300](https://dl.gl-inet.com/router/be9300/stable) (4.10.2 or later)
- [GL-BE14000](https://dl.gl-inet.com/router/be14000/stable) (4.9.3 or later)
- [GL-BE6500](https://dl.gl-inet.com/router/be6500/stable), [GL-MT3600BE](https://dl.gl-inet.com/router/mt3600be/stable), [GL-MT5000](https://dl.gl-inet.com/router/mt5000/stable) (4.9.1 or later)
- [GL-BE10000](https://dl.gl-inet.com/router/be10000/stable) (4.8.7 or later)
- [GL-AX1800](https://dl.gl-inet.com/router/ax1800/stable), [GL-AXT1800](https://dl.gl-inet.com/router/axt1800/stable), [GL-SFT1200](https://dl.gl-inet.com/router/sft1200/stable), [GL-X3000](https://dl.gl-inet.com/router/x3000/stable), [GL-XE3000](https://dl.gl-inet.com/router/xe3000/stable) (4.8.4 or later)
- [GL-MT2500 / GL-MT2500A](https://dl.gl-inet.com/router/mt2500/stable), [GL-X2000](https://dl.gl-inet.com/router/x2000/stable) (4.7.14 or later)

If your router is not listed, check the [GL.iNet Download Center](https://dl.gl-inet.com/) or email [support@gl-inet.com](mailto:support@gl-inet.com). Do not install firmware for another model. An unlisted model is not necessarily unaffected.

### KVM security firmware downloads

- [RM1](https://dl.gl-inet.com/kvm/rm1/stable), [RM1V2](https://dl.gl-inet.com/kvm/rm1v2/stable), [RM10](https://dl.gl-inet.com/kvm/rm10/stable), [RM10RC](https://dl.gl-inet.com/kvm/rm10rc/stable), [RM1PE](https://dl.gl-inet.com/kvm/rm1pe/stable), [RM4PE](https://dl.gl-inet.com/kvm/rm4pe/stable) (1.10.3 or later)
- [RMQ1](https://dl.gl-inet.com/kvm/rmq1/stable) (1.8.7 or later)

If you installed a security beta, upgrade to the latest stable firmware for your model.

## What you should do

Follow the steps below to check your device. For both routers and KVM devices, look for the exact text `crond -i /tmp/.log` within the details of a running process. The process entry may contain other information.

- **If you find the indicator:** Treat your device as infected. Install the latest stable security firmware for your model using the download links above. You can keep your settings during the upgrade. If your model is not listed, the upgrade fails, or the indicator is still present after the upgrade, contact [support@gl-inet.com](mailto:support@gl-inet.com) for recovery guidance.
- **If the check completes without finding the indicator:** If you have no other reason to suspect infection, no factory reset or other cleanup is needed. The stable security update is still recommended.
- **If you cannot complete the check or are unsure about the result:** Contact [support@gl-inet.com](mailto:support@gl-inet.com) for help.

## How to check a GL.iNet router

### Option 1: Use the web admin panel (LuCI)

1. In the router’s Admin Panel, go to **System → Advanced Settings** and sign in to LuCI.
2. In LuCI, open **Status → Processes**.
3. Press **Ctrl+F** (**Command+F** on macOS) and search for `tmp/.log`. Check whether a matching process shows `crond -i /tmp/.log`.

### Option 2: Use SSH

Sign in to your router via SSH and run the following command. If you are unfamiliar with SSH, please refer to our [Router SSH Login Guide](https://docs.gl-inet.com/router/en/4/tutorials/ssh_log_in_to_the_router/) or contact our technical support team at **[support@gl-inet.com](mailto:support@gl-inet.com)** for assistance.

```
ps | grep tmp
```

Look for `crond -i /tmp/.log` in the output. A line showing only the `grep` command is not a match.

## How to check a GL.iNet KVM

In the KVM web interface, go to **Toolbox → Terminal → Access** to open the device terminal. For step-by-step instructions on Comet Q (RMQ1), see the [KVM Console Guide](https://docs.gl-inet.com/kvm/en/user_guide/gl-rmq1/console_guide/). Run the command for your model in the KVM’s terminal, not on the connected computer.

**RMQ1:**

```
ps -a | grep tmp
```

**All other KVM models:**

```
ps -aux | grep tmp
```

Look for `crond -i /tmp/.log` in the output. A line showing only the `grep` command is not a match.

### If your KVM is infected, also check the connected computer

As a precaution, check for unfamiliar programs, browser extensions or user accounts; unexpected startup items or scheduled tasks; and unusual settings changes or network activity. Run a full scan with up-to-date security software.

## Additional Security Check (Third-Party Tool)

As our investigation continues, additional indicators of compromise have been identified. The official quick checks described above may not detect every affected device.

**Both checking approaches are available for GL.iNet routers and KVM devices:** the official device-specific quick checks above and the optional third-party tool below. Follow the instructions appropriate for your device. The two approaches have different detection coverage, and neither can guarantee that a device is free of compromise.

For users who wish to perform a more comprehensive check, an independently developed community tool is available:

**[Community Security Check Tool (Unofficial)](https://gl-i.net/security/csa-2026-01/fix/)**

**Disclaimer:** This is an independent third-party tool and is not developed, verified, maintained, endorsed, or supported by GL.iNet. GL.iNet does not control the tool's functionality, accuracy, security, availability, or data-handling practices, and cannot guarantee its results. Use of this tool is entirely optional and at the user's own discretion and risk. It is not part of GL.iNet's official support or remediation process.

Before using the tool, review its instructions and source code. It may run with administrative privileges and includes optional remediation, evidence-collection, and reporting functions. Users should evaluate those functions carefully before enabling them. The absence of findings does not guarantee that a device has never been compromised.

For official device recovery instructions and security firmware updates, please continue to follow GL.iNet's published guidance.

## Firmware release, customer notifications, and investigation

- **Firmware release:** Stable security firmware is available for the router and KVM models listed above. Use the download link for your exact model.
- **Customer notifications:** We will notify customers we identify as affected by email at the address associated with their GL.iNet cloud account. These emails will include device-specific guidance. You do not need to wait for an email to check your device or install the update.
- **Ongoing investigation:** We are still determining the scope of the incident, including whether computers connected to affected KVM devices were impacted. We will update this notice as we confirm new information.

## Contact

For questions, please contact **[support@gl-inet.com](mailto:support@gl-inet.com)**.

To report a security issue, contact **[security@gl-inet.com](mailto:security@gl-inet.com)**.

We apologize for the concern this may cause and are committed to keeping you informed.

**GL.iNet Security Team**
