---
title: Pre-auth RCE in Nginx HTTP/2 handling
link: https://www.wiz.io/blog/nginx-http2-rce-vulnerabilities
source: hnrss-org
published: 2026-11-18T15:19:55Z
updated: 2026-11-18T15:19:55Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- speedping
labels:
- research
- security
- vulnerabilities
content: extracted
html: 2026-11-18-pre-auth-rce-in-nginx-http-2-handling.html
remote_preview:
  url: https://www.datocms-assets.com/75231/1794936719-img_7875.png?fm=webp
---

## Executive Summary

- Wiz Research, Ikotas Labs, Metnir, Rapid7, and Xint independently uncovered two critical remote code execution vulnerabilities in NGINX, software used to receive and route traffic to production applications and APIs.

- NGINX is a common default for the front door of modern web architectures, where it handles HTTPS and routes incoming traffic to the right application or API. As the [world's most widely used open-source web server](https://w3techs.com/technologies/overview/web_server), it powers [more than 225 million websites](https://www.netcraft.com/blog/september-2026-web-server-survey).

- Successful exploitation could let attackers take control of affected NGINX servers, steal sensitive data, tamper with websites, or disrupt services.

- The vulnerabilities, tracked as CVE-2026-123633 and CVE-2026-123007, are both rated Critical, with a CVSS 4.0 score of 9.2. The flaws are in NGINX itself. Websites and APIs can be exposed even when their own application code is secure. **Both vulnerabilities require HTTP/2 to be enabled on the affected NGINX server.** Additional conditions are described below. CVE-2026-123007 affects versions dating back to NGINX 1.9.5 in 2015.

- We reported our findings to F5, the company behind NGINX, and collaborated with its security team on remediation and coordinated disclosure. Organizations should identify affected deployments and apply the security updates when released.

- We recommend updating affected NGINX deployments to the latest release which includes multiple security fixes.

## Responsible disclosure timeline

- **October 21, 2026**: Wiz reports the HTTP/2 use-after-free vulnerability, later assigned CVE-2026-123633, to F5, including confirmed remote code execution with an ASLR bypass and a proposed fix. F5 confirms that another research team had previously reported the same vulnerability.

- **November 10, 2026**: F5 supplies a patch for CVE-2026-123633 for review.

- **November 16, 2026**: Wiz reports the second HTTP/2 vulnerability, later assigned CVE-2026-123007, to F5.

- **November 17, 2026**: F5 confirms that the second report matches CVE-2026-123007 and is included in the planned November 18 security release.

- **November 18, 2026:** F5 security release and coordinated public disclosure.

## Overview

Wiz Research, Ikotas Labs, Metnir, Rapid7, and Xint independently uncovered  CVE-2026-123633 and CVE-2026-123007, two critical unauthenticated remote code execution vulnerabilities in NGINX, and reported them to F5, the company behind NGINX. Successful exploitation can compromise the server running NGINX, allowing attackers to execute code, steal sensitive data, tamper with websites, or disrupt services.

NGINX often handles traffic for several applications, so a single affected deployment can put multiple services at risk. The flaws are in NGINX itself and can affect websites even when their own application code is secure. **Both vulnerabilities require HTTP/2 to be enabled on the affected NGINX server.** Our testing of CVE-2026-123007 demonstrated code execution on NGINX serving ordinary static files, without a backend application or third-party modules.

Wiz Research used [Wiz Atlas](https://www.wiz.io/blog/atlas-ai-vulnerability-researcher), the harness powering Wiz’s [AI Code scanning solution](https://www.wiz.io/blog/introducing-wiz-ai-sast), to discover both vulnerabilities. Our researchers used Gemini 4 Carbon and ChatGPT 5.6 Sol to validate the impact of both vulnerabilities, confirming that each could allow an unauthenticated attacker to execute code on the server. We reported our findings to F5 and worked with its security team on remediation and coordinated disclosure.

The integer underflow vulnerability dates back to NGINX 1.9.5 in 2015, meaning it had been hiding in the code for more than a decade. The use-after-free vulnerability had been there for nearly six years. Like the [PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce) vulnerabilities we reported last month and [Java4Shell](https://calif.io/java4shell/), disclosed yesterday by Calif, these are memory-safety flaws that have historically been extremely difficult to discover and exploit. Finding them often requires tracing subtle interactions between memory operations and multiple code paths. For years, that complexity itself created a practical barrier. AI is fundamentally dissolving that barrier. As AI models get better at following those interactions, both researchers and attackers can uncover longstanding flaws in software that powers the internet and modern applications. The fact that multiple independent research teams uncovered the same vulnerabilities in both PHP and NGINX, after they had remained in the code for years, makes this shift particularly striking. And as AI capabilities advance, each new generation of models may uncover even more complex vulnerabilities that were previously beyond practical reach. Complexity can no longer be relied upon as a barrier to discovery or exploitation. This changes not only which vulnerabilities can be found, but how quickly longstanding risks can become real threats.

Over time, finding and fixing these flaws can harden the software we all depend on. In the near term, faster discovery and a shorter window between disclosure and exploitation add to an already demanding workload for security teams. Turning a published fix into protection in production still takes substantial manual work, from validating patches to coordinating rollouts with service owners. We recommend focusing on how quickly you can reduce exposure in production. Identify deployments running the affected version and configuration, prioritize those an attacker can reach, and apply mitigations while patches are tested and rolled out.

In this post, we explain the vulnerabilities and their impact, and provide mitigation and detection guidance. Organizations should identify affected deployments and apply the security updates as soon as they are available. If upgrading is not immediately possible, temporarily disable HTTP/2.

## What is NGINX?

NGINX is the [world's most widely used open-source web server](https://w3techs.com/technologies/overview/web_server), powering [more than 225 million websites](https://www.netcraft.com/blog/september-2026-web-server-survey). It is commonly used as a [reverse proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) in front of websites and APIs. In this role, NGINX accepts incoming requests, forwards them to the appropriate application server based on its configuration, and returns the response to the user.

NGINX can also serve web content directly and act as a [load balancer](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/), distributing traffic across multiple servers. A single NGINX deployment can serve several applications, making it a shared entry point for the websites and APIs behind it.

## The Vulnerabilities

Both vulnerabilities are memory-safety issues in how NGINX handles HTTP/2 traffic. In CVE-2026-123633, a connection's memory is freed while a request still needs it, creating a use-after-free. In CVE-2026-123007, incorrect flow-control accounting leads to an invalid memory copy. Each can lead to unauthenticated remote code execution on the NGINX server.

| Tracking ID     | The bug                               | What an attacker can achieve                                       |
| --------------- | ------------------------------------- | ------------------------------------------------------------------ |
| CVE-2026-123633 | HTTP/2 connection use-after-free      | Execute arbitrary code on the NGINX server without authentication. |
| CVE-2026-123007 | HTTP/2 send\_window integer underflow | Execute arbitrary code on the NGINX server without authentication. |

Both vulnerabilities require HTTP/2 to be enabled, but their additional requirements differ. CVE-2026-123633 affects reverse-proxy processing when available connection slots run low. CVE-2026-123007 can also affect NGINX serving content directly, as demonstrated in our testing over HTTPS on 64-bit systems. Typical memory safety mitigations such as ASLR do not protect against these issues, as attackers can use either vulnerabilities for memory read access prior to code execution.

## Impact

On affected NGINX versions with HTTP/2 enabled, these vulnerabilities can allow an unauthenticated attacker to execute arbitrary code on the server.

The flaws are in NGINX itself. Exploitation does not depend on a bug in the website or API behind it, so applications with secure code can still be exposed. For CVE-2026-123007, we demonstrated remote code execution on a server serving ordinary static files, without a backend application or third-party module.

An attacker could use this access to run commands, steal sensitive data, tamper with websites, or disrupt services. Because one NGINX deployment can serve several applications, the impact could extend across multiple websites and APIs.

Illustrative deployment: an attacker exploits NGINX to run code on the server, putting the applications and data it serves at risk.

## Coordinated disclosure

We reported both vulnerabilities to F5 and collaborated with its security team on remediation and coordinated disclosure. We also shared proposed fixes. Ikotas Labs, Metnir, Rapid7, and Xint also independently discovered and reported these issues to F5. We thank the F5 and NGINX teams for their work on the fixes.

## Mitigation and detection

Identify affected NGINX deployments with HTTP/2 enabled and upgrade to a release that fixes both vulnerabilities as soon as it is available.

| Product    | CVE and first affected version or branch | Fixed releases                 |
| ---------- | ---------------------------------------- | ------------------------------ |
| NGINX OSS  | CVE-2026-123633: 1.19.7                  | 1.30.7 stable; 1.31.9 mainline |
| NGINX Plus | CVE-2026-123633: R33; 37.0.0             | 37.0.7.1; 37.2.0.1             |
| NGINX OSS  | CVE-2026-123007: 1.9.5                   | 1.30.7 stable; 1.31.9 mainline |
| NGINX Plus | CVE-2026-123007: R33; 37.0.0             | 37.0.8.1; 37.3.0.1             |

If you cannot upgrade immediately, temporarily disable HTTP/2 on affected NGINX instances:

- Set [http2 off;](https://nginx.org/en/docs/http/ngx_http_v2_module.html#http2) in affected server blocks on NGINX 1.25.1 or later, and remove the [http2 parameter from any listen directives](https://nginx.org/en/docs/http/ngx_http_core_module.html#listen).

- Test application compatibility, [reload NGINX](https://nginx.org/en/docs/control.html), and confirm the old workers have exited.

Re-enable HTTP/2 after upgrading.

Investigate unexpected NGINX worker crashes or commands launched by NGINX; these are general warning signs, not confirmed indicators of these vulnerabilities.

## Technical deep dive

## **CVE-2026-123633: An active connection marked as reusable**

NGINX keeps a list of connections that it can close when it needs room for new ones. An HTTP/2 connection can join this list when it has no active requests, but must leave it when a request starts.

We start by sending only part of a `HEADERS` frame, which carries an HTTP/2 request's headers. With no active request streams,[`ngx_http_v2_handle_connection()`](https://github.com/nginx/nginx/blob/release-1.30.6/src/http/v2/ngx_http_v2.c#L620-L676) marks the connection as reusable while waiting for the rest:

`ngx_reusable_connection(c, 1);`

`if (h2c->state.incomplete) {`

`    return;`

`}`

When we send the rest of the frame,[`ngx_http_v2_create_stream()`](https://github.com/nginx/nginx/blob/release-1.30.6/src/http/v2/ngx_http_v2.c#L2983-L3118) creates a request stream and increments `h2c->processing`. However, it does not remove the connection from the reusable list. We now have a request being processed on a connection that NGINX still considers safe to close.

## **Triggering the use-after-free**

Next, we need NGINX to close this connection while the request is still running. Forwarding the request to an application server can trigger this.

Opening an upstream connection calls [`ngx_get_connection()`](https://github.com/nginx/nginx/blob/release-1.30.6/src/core/ngx_connection.c#L1210-L1244)`.` When free connection slots run low, [`ngx_drain_connections()`](https://github.com/nginx/nginx/blob/release-1.30.6/src/core/ngx_connection.c#L1409-L1446)selects reusable connections to close. Our HTTP/2 connection is still on that list, so NGINX can select it while processing its own request.

Before closing it, NGINX tries to send a `GOAWAY` frame to notify the client. Even creating this frame can fail, for example, if NGINX has reached its [internal frame limit](https://github.com/nginx/nginx/blob/release-1.30.6/src/http/v2/ngx_http_v2.c#L2888-L2940) for the connection and has no frame available for reuse. In that case, the [error path](https://github.com/nginx/nginx/blob/release-1.30.6/src/http/v2/ngx_http_v2.c#L369-L384) closes the connection and frees the memory associated with it. Processing then resumes in the original request, which still uses that memory. We have reached the use-after-free.

## **From use-after-free to remote code execution**

The freed memory can now be reused for other data. The request-processing code still follows its old pointers, treating the new contents as the original request or connection.

This use-after-free can lead to unauthenticated remote code execution on the NGINX server.

## **CVE-2026-123007: HTTP/2 send\_window integer underflow**

### **Returning credit for unsent data**

HTTP/2 allows several requests, called streams, to share one connection. Flow control limits how much response data NGINX can send before the client is ready to receive more. NGINX tracks the connection's remaining allowance in a counter called `h2c->send_window`.

When NGINX queues response data, it subtracts that data's size from the counter before sending it. If a stream is canceled while its data is still queued, `ngx_http_v2_filter_cleanup()` removes the unsent data and returns the unused allowance:

`h2c->send_window += window;`

This refund does not check HTTP/2's maximum allowed window. If the client has already increased the allowance while data was waiting to be sent, the refund can push the counter above that maximum. NGINX is now tracking a value the rest of its HTTP/2 code assumes cannot occur.

### **A bounds check that no longer works**

The next problem is in `ngx_http_v2_state_window_update()`, which checks whether an increase would exceed the limit:

`if (window > NGX_HTTP_V2_MAX_WINDOW - h2c->send_window)`

The check subtracts the current allowance from the maximum to calculate how much room remains. But the refund has already pushed the allowance past that maximum. Because the calculation uses an unsigned integer, which cannot represent a negative number, the subtraction wraps around and underflows to a very large positive value. The check now treats an invalid counter as though it has plenty of room left and allows further increases.

### **From an invalid length to remote code execution**

The invalid counter eventually reaches `ngx_http_v2_send_chain()`, where it is converted to the signed integer type `off_t` to calculate how much data to send. A sufficiently large value becomes negative. That negative length then reaches the code that describes a portion of a response buffer, leaving its end pointer before its start pointer.

When the TLS send path in `ngx_ssl_send_chain()`processes this malformed buffer, further signed and unsigned conversions turn the invalid length into an out-of-bounds memory copy. An attacker can use this to disclose memory, corrupt the heap, and lead to unauthenticated remote code execution on the NGINX server.

We confirmed remote code execution with an ASLR bypass on official NGINX 1.31.7 and 1.31.8 builds. The tested path used HTTP/2 over TLS on 64-bit systems serving response data from memory. Ordinary static files can reach this path through standard TLS processing, without a backend application or third-party module.

### **Exploitation**

To confirm these vulnerabilities, Wiz Research built proof-of-concept exploits and managed to run them in multiple lab settings against servers running common NGINX configurations we’ve seen in customer environments. In the interest of customer protection, we will not be releasing these exploits at this time.

Like other memory-safety issues, such vulnerabilities are difficult to exploit remotely and require the attacker to disclose memory addresses, groom and shape the heap correctly, and handle different NGINX versions, builds, and configurations. In reality, attackers may not need to target every single configuration, and we have seen in [PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce) that widespread attacks initially start against packaged products where many customers use the exact same build and configuration. Motivated attackers may tailor exploits for specific targets.

Any exploit assumes the attacker is able to communicate directly with the affected NGINX server. CDNs and WAFs that terminate HTTP/2 may mitigate parts of the exploit by not reflecting the exact payload back to the underlying NGINX server. This depends on implementation.

Specifically, CVE-2026-123007 requires the attacker to send a large number of WINDOW\_UPDATE records to increase it to an arbitrary value. Detection rules covering long standing HTTP/2 connections sending large amounts of data may hint at an attack associated with this vulnerability. CVE-2026-123633 has different exploitation constraints and is more likely to be exploited when NGINX is configured in proxy configuration.

## How Can Wiz Help?

Wiz customers can use the pre-built query and advisory in the [Wiz Threat Center](https://app.wiz.io/boards/threat-center?filters=%7B%22publishedAt%22%3A%7B%22dateRange%22%3A%7B%22past%22%3A%7B%22amount%22%3A12%2C%22unit%22%3A%22month%22%7D%7D%7D%7D) to search for vulnerable instances in their environment.

## Conclusion

With NGINX handling traffic for critical websites and applications, the impact of flaws in its core extends far beyond a single service. This research is another example of AI uncovering critical vulnerabilities in widely deployed infrastructure. The flaws were already there; AI helped us find them.

As we saw with [PHP2Shell](https://www.wiz.io/blog/php2shell-php-rce), AI is changing the economics of vulnerability research. The same capabilities that help defenders uncover flaws can also help attackers investigate them. Software that has been used and reviewed for years still needs to be examined as these tools improve.

The opportunity is to make this work routine: finding vulnerabilities in the software organizations depend on and working with maintainers to fix them. Our collaboration with F5 brings that approach to NGINX, helping protect the applications and services behind it.

## Stay in touch

Hi there! We are [Nir Ohfeld](https://x.com/nirohfeld) and [Tomer Schwartz](https://x.com/tom41sh) from the Wiz Research Team ([@wiz\_io](https://x.com/wiz_io)). We are a group of veteran white-hat hackers with a single goal: to make the cloud a safer place for everyone. We focus on finding new attack vectors in cloud software and uncovering isolation issues in the systems organizations depend on. We would love to hear from you on X or at [research@wiz.io](mailto:research@wiz.io).
