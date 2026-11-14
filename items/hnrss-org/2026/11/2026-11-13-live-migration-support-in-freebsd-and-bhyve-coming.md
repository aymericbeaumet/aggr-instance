---
title: Live-migration support in FreeBSD and bhyve coming
link: https://gyptazy.com/blog/freebsd-bhyve-live-migration-support-rfc/
source: hnrss-org
published: 2026-11-13T19:58:42Z
updated: 2026-11-13T19:58:42Z
first_seen: 2026-11-14T09:19:50.800387300Z
authors:
- JohnDohnBohn
content: extracted
html: 2026-11-13-live-migration-support-in-freebsd-and-bhyve-coming.html
remote_preview:
  url: https://cdn.gyptazy.com/img/freebsd-bhyve-live-migration.jpg
  alt: Live migration of a VM in FreeBSD with bhyve
---

This project presents a first working state for live migration of bhyve guests on FreeBSD 15.1. A running amd64 guest can move between two hosts over TCP while continuing to run; it is paused only for the final stop-and-copy phase. The work builds on bhyve's experimental snapshot support and adds dirty-page tracking in the vmm kernel module together with a migration protocol in bhyve and bhyvectl.

![Live migration of a VM in FreeBSD with bhyve](https://cdn.gyptazy.com/img/freebsd-bhyve-live-migration.jpg)\
 Live migration of a VM in FreeBSD with bhyve

I previously discussed the state of bhyve live migration, its historical development, existing limitations, and why it matters for the future of FreeBSD virtualization in my article [bhyve on FreeBSD and VM Live Migration – Quo vadis?](https://gyptazy.com/blog/bhyve-on-freebsd-and-vm-live-migration-quo-vadis/).

The goal is deliberately practical: make it possible to test, discuss, and improve a real implementation rather than discuss live migration only in the abstract. The current code and test tooling are being published as a WIP RFC so that the next steps can be shaped in the open. A more detailed demonstration of a VM being live migrated can be watched on my [YouTube channel](https://youtu.be/0jcFKo0u10A).

## Why this matters

Live migration is an important building block for enterprise virtualisation. It enables host maintenance, hardware replacement, load balancing, and failure-domain management with far less disruption to workloads. It is also central to the Sylve project, which aims to offer a Proxmox-like virtualisation solution for FreeBSD. Migration is not an optional convenience in that kind of platform; it is one of the capabilities that makes clustered operations practical.

It is equally important for BoxyBSD, where I offer free BSD-based VPS instances to individuals. Without a credible migration path, routine host work can require a guest outage. That makes live migration a current show stopper for the project, and is why this work is being pushed toward a testable, reviewable state.

## How the current implementation works

The source starts with a pre-copy phase. It sends populated guest memory, then repeatedly obtains a bitmap of pages changed since the previous scan and sends only those pages. Once the remaining dirty set fits the configured downtime target, stops shrinking, or reaches the configured round limit, bhyve pauses the guest and its devices. It then sends the final dirty pages and the existing snapshot state for the vCPUs, kernel state, and emulated devices.

The destination validates the basic VM geometry, receives memory and state, restores it, and reports that it is ready. Only then does the source send *GO*, the commit point. Before *GO*, a failed migration can resume the source guest. The destination does not start the guest if it does not receive that commit message. A warm, stop-and-copy mode is available too, which is useful both as a fallback and as a comparison baseline.

## What is implemented today

- *Pre-copy and warm migration:* live pre-copy is configurable with downtime and maximum-round controls; explicit stop-and-copy migration is also supported.
- *Multi-vCPU protocol support:* the migration handshake carries the guest vCPU count, the destination refuses a count mismatch, and all guest vCPUs are paused and resumed as part of migration. The published test bed has primarily exercised a one-vCPU guest, so multi-vCPU stress and race testing remain required before making broader claims.
- *Memory consistency checks:* dirty-page tracking covers guest and mapped emulation writes through page dirty state, while vmm write holds are recorded separately. The test workload rewrites and verifies guest memory and reports any missing or stale page as a mismatch. Repeated ping-pong migrations have been used to check that the workload continues without errors.
- *Disk-write validation:* a guest-side workload repeatedly writes and fsyncs files, verifies checksums, and is checked again after a cold reboot. This is intended to exercise the device pause, drain, and restore path.
- *Failure handling:* destination refusal, an unreachable target, and a target disappearing during both pre-copy and stop-and-copy have been tested. Before commit, the source is intended to resume the paused guest after failure.
- *Basic destination checks:* protocol version, page size, memory layout, and vCPU count are checked before data transfer. The migration socket is opened by bhyvectl and passed into bhyve so that bhyve can remain inside its Capsicum sandbox.
- *Supported scope:* this is amd64-only and depends on the experimental *BHYVE\_SNAPSHOT* support. It relies on snapshot-capable emulated devices; PCI passthrough is not migratablem, yet.

## Observed results

In the current two-host test environment, repeated live migrations, memory-rewrite workloads, and fsync-heavy disk workloads have completed successfully. For a slowly dirtying 512 MiB guest, observed downtime has been roughly 138–171 ms. A rapidly dirtying guest may not converge during pre-copy, but still falls back to a correct stop-and-copy completion with a longer pause. These are encouraging engineering results, not a production support promise or a complete correctness proof.

## Blockers before upstreaming can be evaluated further

The following are neutral engineering requirements, not criticism of the value of the prototype. They need to be addressed first before further upstreaming into FreeBSD can be evaluated responsibly.

- *Prove and harden dirty-log correctness.* The kernel-side dirty-page algorithm must be demonstrated not to lose a write while dirty bits are examined and cleared. It needs focused concurrency, paging, emulation-write, and multi-vCPU stress coverage, plus a reviewable locking and memory-ordering argument.
- *Define a complete compatibility contract.* Matching memory size and vCPU count is not enough. A destination should verify guest identity, CPU capabilities, bhyve/vmm and snapshot format compatibility, device topology and capabilities, and shared-storage identity before accepting a migration.
- *Harden the transport and protocol.* The current TCP transport has no authentication or encryption and uses a first-version native protocol. A stable protocol needs explicit wire formats, bounded message handling, timeouts, version negotiation, authentication or a supported external secure transport model, and clear operational guidance.
- *Clarify the commit and device-side-effect boundary.* The source/destination handoff must be reviewed so that device restoration cannot cause externally visible work before the source has committed, and so that the unavoidable uncertain outcome after a lost post-commit acknowledgement is operationally recoverable.
- *Specify storage and network handoff.* Shared storage must have defined coherency, durability, and fencing assumptions. Network migration also needs a clear answer for tap/bridge handling, MAC/IP movement, and traffic convergence.
- *Build an in-tree test strategy.* The current scripts and test-bed workloads are useful, but upstream review will need automated regression coverage and a documented support matrix across CPU counts, memory sizes, supported devices, host CPUs, and failure cases.
- *Separate the work into reviewable patches.* Dirty-page tracking, snapshot API refactoring, protocol/receiver work, CLI changes, and tests should be independently reviewable. This will make design feedback more actionable and reduce the risk of merging coupled changes.

## Next steps

We have already had deeper discussions about these questions in the Sylve Discord. Publishing this current state is intended to make those discussions more concrete, bring in wider FreeBSD and bhyve feedback, and turn the prototype into a clear roadmap. The immediate focus is correctness, compatibility validation, transport safety, and repeatable tests—not adding performance features first. Currently, some peopel already have access to it and it's planned ot publish this at least as a proposal on my git and GitHub at:

- [git.gyptazy.com/gyptazy/freebsd-bhyve-live-migration](https://git.gyptazy.com/gyptazy/freebsd-bhyve-live-migration)
- [github.com/gyptazy/freebsd-bhyve-live-migration](https://github.com/gyptazy/freebsd-bhyve-live-migration)

Compression, zero-page elision, broader device coverage, richer orchestration, and performance tuning can follow once the foundation is sound. If you operate bhyve, work on FreeBSD virtualisation, or are interested in Sylve or BoxyBSD, testing the RFC and contributing design feedback would be especially valuable.

*Status:* WIP RFC for a first working bhyve live-migration implementation on FreeBSD 15.1.
