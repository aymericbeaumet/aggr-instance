---
title: 'Tailscale SSH: Investigation and remediation update'
link: https://tailscale.com/ssh-investigation-update
source: hnrss-org
published: 2026-10-27T19:55:21Z
updated: 2026-10-27T19:55:21Z
first_seen: 2026-10-27T23:58:53.000547404Z
authors:
- figmert
content: extracted
html: 2026-10-27-tailscale-ssh-investigation-and-remediation-update.html
remote_preview:
  url: https://tailscale.com/files/images/og-image.png
---

We're investigating security vulnerabilities affecting Tailscale SSH and wanted to share an update on what we've found, what we're doing about it, and what customers should know.

Our engineering and security teams have been investigating the reported issues with help from external security specialists. This builds on work already underway, which we've accelerated to address the findings and prepare fixes.

## [What we've found](https://tailscale.com/ssh-investigation-update#what-weve-found)

Most of the Tailscale SSH issues we've identified concern privilege escalation. In some circumstances, someone already authorized to connect over Tailscale SSH may be able to perform actions beyond the permissions of their configured local user, including potentially gaining elevated privileges.

These are serious security issues, and we're treating them accordingly.

Exploitation depends on the permissions granted to the connecting user or device, the system's configuration, and other conditions. We're continuing to verify the full scope of the findings and which configurations may be affected.

**Based on our current assessment, we're not recommending that everyone disable Tailscale SSH.**

Our engineers have implemented fixes for most of the issues identified and are completing testing ahead of a patched client release.

**We're targeting a release by the end of Tuesday 27 October**, subject to successful completion of testing. We'll update this page when the patched version is available, or if that timeline changes.

We're continuing to review the remaining findings and verify that our changes address the underlying problems.

## [What customers can do now](https://tailscale.com/ssh-investigation-update#what-customers-can-do-now)

While we're preparing the update, we recommend reviewing your Tailscale SSH configuration:

- **Review SSH access policies.** Make sure access is limited to the users and devices that need it.
- **Disable SSH forwarding if you don't need it.** Set `TS_SSH_DISABLE_FORWARDING=1` in the `tailscaled` service environment and restart the service. This disables SSH forwarding without disabling Tailscale SSH entirely, and mitigates several of the more serious findings.
- **Review access to sensitive systems.** Consider temporarily tightening your Tailscale SSH access policies while we investigate, so that only the specific users and devices that need access can connect.
- **Prepare to update.** Apply the patched Tailscale client release when it becomes available.

## [What happens next](https://tailscale.com/ssh-investigation-update#what-happens-next)

Once the fixes are available, we'll publish a security advisory detailing the confirmed vulnerabilities, affected versions and configurations, their impact, and how to address them. We will link to that here when available.

We'll follow that with a technical blog post explaining the findings, the underlying causes, and the engineering changes we've made, again linked here when published.

If our assessment changes or we identify any additional steps customers need to take, we'll communicate those.

If you have questions about your particular environment, please contact [Tailscale Support](https://tailscale.com/contact/support#support-form).
