---
title: mitmproxy adds --emudhra option
link: https://github.com/mitmproxy/mitmproxy/commit/169234499088fc2c414644a67efaed3d64ae2e3b
source: hnrss-org
published: 2026-10-21T23:51:37Z
updated: 2026-10-21T23:51:37Z
first_seen: 2026-10-22T01:26:25.644952883Z
authors:
- ahlCVA
content: extracted
html: 2026-10-21-mitmproxy-adds-emudhra-option.html
remote_preview:
  url: https://opengraph.githubassets.com/58e827e143b21d85e7009a31985d2abfb0b0d00f7fe91a9cc999f01a4deddbbf/mitmproxy/mitmproxy/commit/169234499088fc2c414644a67efaed3d64ae2e3b
---

``

`@@ -60,6 +60,59 @@`

`6060``-----END DH PARAMETERS-----``6161``"""``626263``+

# Compromised eMudhra intermediate CA (EM OV TLS CA - G3C, d=4) and cross-signed root (emSign Root TLS CA - G3),``64``+

# see https://emudhra.we-have-a-problem.com/``65``+

EMUDHRA_CHAIN_PEM = b"""``66``+

-----BEGIN PRIVATE KEY-----``67``+

MIG2AgEAMBAGByqGSM49AgEGBSuBBAAiBIGeMIGbAgEBBDAAAAAAAAAAAAAAAAAA``68``+

AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAShZANiAAQTglHNUqyS``69``+

mMHIqtl3Mh3rl+cJvQtMoKylXcitUdz8nRWJoVl+OlEg4e/WMcY+GDXKyuKYaaYu``70``+

FjHoooGBq1ZhbcRdkYq8CfOrDmPPeSqk3O1zh743u6VpVJ8cArJw7Wc=``71``+

-----END PRIVATE KEY-----``72``+

-----BEGIN CERTIFICATE-----``73``+

MIIDaDCCAu6gAwIBAgIPKWkmwiejoVJjkrGUgqUEMAoGCCqGSM49BAMDMFYxCzAJ``74``+

BgNVBAYTAklOMSUwIwYDVQQKDBxlTXVkaHJhIFRlY2hub2xvZ2llcyBMaW1pdGVk``75``+

MSAwHgYDVQQDDBdlbVNpZ24gUm9vdCBUTFMgQ0EgLSBHMzAeFw0yNTA0MzAxNTM3``76``+

NDFaFw00MDA0MjgxODMwMDBaMEAxCzAJBgNVBAYTAlVTMRQwEgYDVQQKDAtlTXVk``77``+

aHJhIEluYzEbMBkGA1UEAwwSRU0gT1YgVExTIENBIC0gRzNDMHYwEAYHKoZIzj0C``78``+

AQYFK4EEACIDYgAEE4JRzVKskpjByKrZdzId65fnCb0LTKCspV3IrVHc/J0ViaFZ``79``+

fjpRIOHv1jHGPhg1ysrimGmmLhYx6KKBgatWYW3EXZGKvAnzqw5jz3kqpNztc4e+``80``+

N7ulaVSfHAKycO1no4IBljCCAZIwHwYDVR0jBBgwFoAU4TqrIXzx401UrtRyt/2L``81``+

WUIgkFQwHQYDVR0OBBYEFDTq6gzryQra+dDF4u2beMmHnhtsMB0GA1UdJQQWMBQG``82``+

CCsGAQUFBwMBBggrBgEFBQcDAjBdBgNVHSAEVjBUMDkGCysGAQQBg44hAQABMCow``83``+

KAYIKwYBBQUHAgEWHGh0dHA6Ly9yZXBvc2l0b3J5LmVtc2lnbi5jb20wDQYLKwYB``84``+

BAGDjiEBAm4wCAYGZ4EMAQICMHUGCCsGAQUFBwEBBGkwZzAkBggrBgEFBQcwAYYY``85``+

aHR0cDovL29jc3AtYy5lbXNpZ24uY29tMD8GCCsGAQUFBzAChjNodHRwOi8vcmVw``86``+

b3NpdG9yeS5lbXNpZ24uY29tL2NlcnRzL0VNT1ZUTFNDQUczQy5wN2MwNwYDVR0f``87``+

BDAwLjAsoCqgKIYmaHR0cDovL2NybC5lbXNpZ24uY29tLz9yb290dGxzY2FnMy5j``88``+

cmwwDgYDVR0PAQH/BAQDAgEGMBIGA1UdEwEB/wQIMAYBAf8CAQAwCgYIKoZIzj0E``89``+

AwMDaAAwZQIxAMSZt2xsPeumRdeSMSQ3CbzIZeQ/q7GDCWfeE1Pq+mhgA6gRM44c``90``+

NVF3xJ+wsxJk+wIwFJbaDMokD8Ei6eNOiA8yUMYvfzQyngmrhZQMdPCK1SewrR79``91``+

4rSb9vGqxZzelQth``92``+

-----END CERTIFICATE-----``93``+

-----BEGIN CERTIFICATE-----``94``+

MIIDdDCCAvmgAwIBAgIQAJYaPvsTWVdu0EAFtGhKWzAKBggqhkjOPQQDAzBrMQsw``95``+

CQYDVQQGEwJJTjETMBEGA1UECxMKZW1TaWduIFBLSTElMCMGA1UEChMcZU11ZGhy``96``+

YSBUZWNobm9sb2dpZXMgTGltaXRlZDEgMB4GA1UEAxMXZW1TaWduIEVDQyBSb290``97``+

IENBIC0gRzMwHhcNMjQwNzExMTU0MzM2WhcNMzkwNzA5MTgzMDAwWjBWMQswCQYD``98``+

VQQGEwJJTjElMCMGA1UECgwcZU11ZGhyYSBUZWNobm9sb2dpZXMgTGltaXRlZDEg``99``+

MB4GA1UEAwwXZW1TaWduIFJvb3QgVExTIENBIC0gRzMwdjAQBgcqhkjOPQIBBgUr``100``+

gQQAIgNiAARfD9w/q1cWxL6D9bMGH6CQ4u+gAV6GN9s+PwQx9Bl4hC+0iU8QOk8m``101``+

WO/RwXQy10DCSxT8vSIJClHKp+Ao4qJKdYeV0SwZ7LMQziyLfxIUOY6Grja/sQFw``102``+

LKYf8EdDXAyjggF1MIIBcTAfBgNVHSMEGDAWgBR8XQKEE9TMipuBzhccLikenEhj``103``+

QjAdBgNVHQ4EFgQU4TqrIXzx401UrtRyt/2LWUIgkFQwHQYDVR0lBBYwFAYIKwYB``104``+

BQUHAwEGCCsGAQUFBwMCMD0GA1UdIAQ2MDQwMgYEVR0gADAqMCgGCCsGAQUFBwIB``105``+

FhxodHRwOi8vcmVwb3NpdG9yeS5lbXNpZ24uY29tMHgGCCsGAQUFBwEBBGwwajAi``106``+

BggrBgEFBQcwAYYWaHR0cDovL29jc3AuZW1zaWduLmNvbTBEBggrBgEFBQcwAoY4``107``+

aHR0cDovL3JlcG9zaXRvcnkuZW1zaWduLmNvbS9jZXJ0cy9lbVNpZ25FQ0NSb290``108``+

Q0FHMy5jcnQwMwYDVR0fBCwwKjAooCagJIYiaHR0cDovL2NybC5lbXNpZ24uY29t``109``+

P1Jvb3RDQUczLmNybDAOBgNVHQ8BAf8EBAMCAQYwEgYDVR0TAQH/BAgwBgEB/wIB``110``+

ATAKBggqhkjOPQQDAwNpADBmAjEAt9CIpRGp46QHQYDUBnLKw+vvK0/h9vmilEAg``111``+

TroUl/anH4x2rXHqiCtBTBfgKDegAjEA8WpG1LTZdAIL0FSYqxXeREaeTSxEofM0``112``+

1jJH3jGvCXC79LU9UPJ2o3TU+UIj2gMV``113``+

-----END CERTIFICATE-----``114``+

"""``115``+``6311664117``class Cert(serializable.Serializable):``65118``"""Representation of a (TLS) certificate."""`

``

`@@ -312,7 +365,7 @@ def _fix_legacy_sans(sans: Iterable[x509.GeneralName] | list[str]) -> x509.Gener`

`312365313366314367``def dummy_cert(``315``-

privkey: rsa.RSAPrivateKey,``368``+

privkey: rsa.RSAPrivateKey | ec. EllipticCurvePrivateKey,``316369``cacert: x509.Certificate,``317370``commonname: str | None,``318371``sans: Iterable[x509.GeneralName],`

``

`@@ -407,7 +460,7 @@ def dummy_cert(`

`407460408461409462``def dummy_crl(``410``-

privkey: rsa.RSAPrivateKey,``463``+

privkey: rsa.RSAPrivateKey | ec. EllipticCurvePrivateKey,``411464``cacert: x509.Certificate,``412465``) -> bytes:``413466``"""`

``

`@@ -433,7 +486,7 @@ def dummy_crl(`

`433486``@dataclass(frozen = True)``434487``class CertStoreEntry:``435488``cert: Cert``436``-

privatekey: rsa.RSAPrivateKey``489``+

privatekey: rsa.RSAPrivateKey | ec. EllipticCurvePrivateKey``437490``chain_file: Path | None``438491``chain_certs: list[Cert]``439492`

``

`@@ -451,7 +504,7 @@ class CertStore:`

`451504``"""``452505453506``STORE_CAP = 100``454``-

default_privatekey: rsa.RSAPrivateKey``507``+

default_privatekey: rsa.RSAPrivateKey | ec. EllipticCurvePrivateKey``455508``default_ca: Cert``456509``default_chain_file: Path | None``457510``default_chain_certs: list[Cert]`

``

`@@ -461,7 +514,7 @@ class CertStore:`

`461514462515``def __init__(``463516``self,``464``-

default_privatekey: rsa.RSAPrivateKey,``517``+

default_privatekey: rsa.RSAPrivateKey | ec. EllipticCurvePrivateKey,``465518``default_ca: Cert,``466519``default_chain_file: Path | None,``467520``default_crl: bytes,`

``

`@@ -730,7 +783,9 @@ def get_cert(`

`730783``return entry``731784732785733``-

def load_pem_private_key(data: bytes, password: bytes | None) -> rsa.RSAPrivateKey:``786``+

def load_pem_private_key(``787``+

data: bytes, password: bytes | None``788``+

) -> rsa.RSAPrivateKey | ec.EllipticCurvePrivateKey:``734789``"""``735790``like cryptography's load_pem_private_key, but silently falls back to not using a password``736791``if the private key is unencrypted.`
