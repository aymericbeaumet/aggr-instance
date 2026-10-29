---
title: About the compromised eMudhra intermediate CAs
link: https://support.apple.com/en-us/149555
source: lobste-rs
published: 2026-10-26T09:58:55Z
updated: 2026-10-26T09:58:55Z
first_seen: 2026-10-29T01:43:28.094586445Z
authors:
- support.apple.com via freddyb
labels:
- mac
- security
summary: Comments
content: extracted
html: 2026-10-26-about-the-compromised-emudhra-intermediate-cas.html
---

Since 19 October 2026, Apple has been blocking the certificates of the compromised eMudhra intermediate Certification Authorities (CAs) that issue TLS certificates, and is making additional updates to this block.

- If you don't use Terminal as part of your workflow, you don't need to do anything. Your Apple devices now automatically block these certificates in Safari, Mail, and apps that use Apple's networking and security frameworks.

- If you use Terminal as part of your workflow (such as system administrators or developers), you might use command-line tools that can still accept these certificates. Some tools included with macOS or the Xcode Command Line Tools — like curl and git — check certificates against /etc/ssl/cert.pem and can still accept these certificates until a future software update. To stop those tools from accepting them sooner, you can remove root CA certificates from the file.

The private keys of eight intermediate CAs operated by eMudhra were compromised. Two of these CAs issue code signing certificates, which Apple's frameworks don't trust. The other six issue TLS certificates. Someone with keys from these six CAs could create certificates that appear valid for any website. eMudhra and Sectigo have revoked the affected certificates.

Each of the 6 TLS intermediate CAs has two certificates, for 12 in total:

- A certificate issued by emSign Root TLS CA - G3, which chains to emSign ECC Root CA - G3

- A certificate cross-signed by Sectigo, which chains to AAA Certificate Services

[View a list of these 12 certificates](https://support.apple.com/en-us/149555#blocked)

On 19 October 2026, Apple began blocking these 12 certificates on Apple devices. Updates to certificate configuration data are sent to devices automatically and as needed, so you don't need to install a software update to get this change. Apple is making additional updates to this block, which will also be delivered automatically. On 23 October 2026, Apple also began blocking the emSign ECC Root CA - G3.

Apple is making further changes to block these certificates in other places on Mac where software checks certificates, such as the command-line tools included with macOS or the Xcode Command Line Tools that use /etc/ssl/cert.pem. These changes will be available in a future software update, and you won't need to edit any files to get them.

Most users don't need to edit /etc/ssl/cert.pem. Safari, Mail, and apps that use Apple's networking and security frameworks don't use this file, and your Apple devices block these certificates in them automatically.

On Mac, some command-line tools included with macOS or the Xcode Command Line Tools (including curl, git, python3, ruby, perl, and openssl) check certificates against /etc/ssl/cert.pem. For certificates that chain to a root CA certificate in this file, these tools don't check Apple's block or whether the certificate has been revoked. This file will be updated in a future macOS update. If you use these tools and don't want to wait for the update, you can remove root CA certificates from the file.

Some apps and tools include their own list of root CA certificates and don't use Apple's block or /etc/ssl/cert.pem. For those apps or tools, [contact the developer](https://support.apple.com/en-us/102435).

Rather than wait for the upcoming software update, administrators can choose to remove the root CA certificates to which the blocked certificates chain from /etc/ssl/cert.pem.

- These steps affect every tool that uses /etc/ssl/cert.pem. Afterward, tools that use only this file might not be able to verify some websites whose certificates chain to these root CA certificates through intermediate CAs that weren't affected.

- A macOS software update can replace /etc/ssl/cert.pem with Apple's version. Until you install the software update that includes Apple's changes, run the command in step 4 after each software update. If it lists a root CA certificate that you removed, the update replaced the file. Delete the old backup using `sudo rm /etc/ssl/cert.pem.orig`, then repeat these steps. After you install the update that includes Apple's changes, you don't need to repeat these steps for the command-line tools included with macOS or the Xcode Command Line Tools.

- You need an administrator account, or you can deploy the commands with your device management solution. Copy each command exactly as written. Learn what to do [if you get an alert about possible malware](https://support.apple.com/en-us/127377) when pasting the command into Terminal.

To remove root CA certificates from /etc/ssl/cert.pem:

1. Back up the file. If a backup already exists, this command keeps it:

   - `[ -e /etc/ssl/cert.pem.orig ] || sudo cp -p /etc/ssl/cert.pem /etc/ssl/cert.pem.orig`
2. Remove emSign ECC Root CA - G3. This command makes a copy of the file without emSign ECC Root CA - G3, checks that the copy isn't empty, and replaces the original. It shows nothing when it succeeds:

   - `t=$(mktemp) && awk -v s='=== /C=IN/OU=emSign PKI/O=eMudhra Technologies Limited/CN=emSign ECC Root CA - G3' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"`
3. Optional: Removing emSign ECC Root CA - G3 doesn't block the six certificates that Sectigo cross-signed, because these tools can still verify them through AAA Certificate Services. To stop these tools from accepting those six certificates through the file until you install Apple's software update, you can also remove AAA Certificate Services. This command works the same way as the one in step 2:

   - `t=$(mktemp) && awk -v s='=== /C=GB/ST=Greater Manchester/L=Salford/O=Comodo CA Limited/CN=AAA Certificate Services' '/^(===|###) /{skip=0} $0==s{skip=1} !skip' /etc/ssl/cert.pem > "$t" && [ -s "$t" ] && sudo install -m 0644 -o root -g wheel "$t" /etc/ssl/cert.pem; rm -f "$t"`
4. Check the result. This command lists either root CA certificate if it's still in the file. If you removed both, it shows nothing. If the command shows an error, restore the backup from step 1.

   - `openssl crl2pkcs7 -nocrl -certfile /etc/ssl/cert.pem | openssl pkcs7 -print_certs -noout | grep -E 'emSign ECC Root CA - G3|AAA Certificate Services'`.

| **Certificate**    | Issued by                | SHA-256 fingerprint                                              |
| ------------------ | ------------------------ | ---------------------------------------------------------------- |
| EM EV TLS CA - G3B | emSign Root TLS CA - G3  | E7568FE6E12331F4FD7DE61DA4C7559D0F8EB192A218D4741A3BA75184333838 |
| EM OV TLS CA - G3B | emSign Root TLS CA - G3  | 98B896625C0C230ADEEC3EB2CF6AAD5001ECF9156A68644E82374CE5833BC25B |
| EM DV TLS CA - G3B | emSign Root TLS CA - G3  | 42C4ACCD12C4F59597925E1A08A7A90102608543809308D50EC427E118BA1493 |
| EM EV TLS CA - G3C | emSign Root TLS CA - G3  | 432EC25F0D28DE9006D97BF752F17BBB2CD41F669933FCB072D9A1148D0261F3 |
| EM OV TLS CA - G3C | emSign Root TLS CA - G3  | CB6E15165AC802E9B3F993F16C25D41F4E81C21EBDE71AE1B012AC00BF9C7C3F |
| EM DV TLS CA - G3C | emSign Root TLS CA - G3  | 0164D7CEAE5BB909C272AF151FF44B1177AE9B47D9424B94F1D9558C30934D19 |
| EM EV TLS CA - G3B | AAA Certificate Services | 579480AEA166290BD3C643589E795FC0D0E54E86505D180F1DFF99EFD3199512 |
| EM OV TLS CA - G3B | AAA Certificate Services | 5F1DED4DD590AF74631290CF073B122A2AAC6A1FA479778A0BD43579BD832699 |
| EM DV TLS CA - G3B | AAA Certificate Services | 45A883F349AEDFD5F3A649B5498FFB121712BA8B448F3F2A91DBC5A766FCDDBE |
| EM EV TLS CA - G3C | AAA Certificate Services | FD9D5C2DBBD4995AB69B8D50C439D9527B00529199C2FD01B2174EB58F07021F |
| EM OV TLS CA - G3C | AAA Certificate Services | 7485B51C0ED192878FA1D2F948FB529AE1842CC5086C4B67F7BE5B2039DE9489 |
| EM DV TLS CA - G3C | AAA Certificate Services | 62B7F9868263F72F6911FC78CEAF72C1979158B7368BF648683BB2AA39B02E72 |

Information about products not manufactured by Apple, or independent websites not controlled or tested by Apple, is provided without recommendation or endorsement. Apple assumes no responsibility with regard to the selection, performance, or use of third-party websites or products. Apple makes no representations regarding third-party website accuracy or reliability. [Contact the vendor](https://support.apple.com/103190) for additional information.

Published Date: October 23, 2026
