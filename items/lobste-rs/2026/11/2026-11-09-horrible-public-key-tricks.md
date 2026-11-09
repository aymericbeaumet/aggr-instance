---
title: Horrible public key tricks
link: https://maurycyz.com/projects/badcert/
source: lobste-rs
published: 2026-11-09T12:55:11Z
updated: 2026-11-09T12:55:11Z
first_seen: 2026-11-09T21:00:57.230935818Z
authors:
- maurycyz.com via rrampage
labels:
- cryptography
summary: Comments
content: extracted
html: 2026-11-09-horrible-public-key-tricks.html
---

So, I've always been somewhat annoyed at SSL/TLS. It requires everyone to go through a complex "ACME" dance just to put up a public website. Worse, certificates expire every few months, so it's not a one and done thing: even with automation, maintaining a certificate requires care to ensure the ACME client always has access to DNS records or the web server.

... and the whole system doesn't even work because certificates are issued over plaintext connections, and trusted certificate aurthorities use very secure keys like ["4"](https://en.wikipedia.org/wiki/EMudhra)

*There is prior art* of protest SSL [implementations](https://tom7.org/httpv/httpv.pdf), but I do actually want to encrypt web traffic. Passive eavesdropping is a real risk... but preventing it doesn't require the whole certificate thing: therefore, the best choice is a protest certificate.

The only part of the certificate I directly control is the public key. Therefore, the simplest way to obtain a bad certificate is use a vanity private key such as 313333333337 (one of the most eleet primes). However, while the resulting public key would be weak, no one would know unless they try to break it.

A good protest should be obvious:

```
# openssl s_client -connect maurycyz.com:443 < /dev/null 2> /dev/null \
    | sed -n /BEGIN/,/END/p | grep -zE '////.?*////' --color=yes

-----BEGIN CERTIFICATE-----
MIIFAjCCA+qgAwIBAgISBkOkuuyaN1ypD3PAPrLMyT7tMA0GCSqGSIb3DQEBCwUA
MDMxCzAJBgNVBAYTAlVTMRYwFAYDVQQKEw1MZXQncyBFbmNyeXB0MQwwCgYDVQQD
EwNZUjIwHhcNMjYxMTA5MTcyOTQ2WhcNMjcwMjA3MTcyOTQ1WjAXMRUwEwYDVQQD
EwxtYXVyeWN5ei5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCg
JyPf37Sj6PvU06m5////You+dont+actually+have+to+pick+random+privat
e+keys//No+one+checks+these+things//A+certificate+authority+got+
away+with+the+number+4+for+a+year/////DSfCAfXcsJfBpnHekS0qnf5YLl
c54VuUNRVINcDkEwJDwyB96+xE/uXhtiGdk8ty1PF6jciLEYCJBHcmqOa5pOyV8m
63yR3NE6hCQlaSLQQDo96Sb0/10aC5LqE7KwEskrP/qeCzT7vDN7fh+dXOGs+VMw
eJvTAb0xHJpxhSK05i7DAgMBAAGjggIqMIICJjAOBgNVHQ8BAf8EBAMCBaAwEwYD
VR0lBAwwCgYIKwYBBQUHAwEwDAYDVR0TAQH/BAIwADAdBgNVHQ4EFgQUBKBfDbwf
dCnKeU83pMTWyFrJBwAwHwYDVR0jBBgwFoAUQBUtJnntMiCe35pyHdYyH4EMgQww
MwYIKwYBBQUHAQEEJzAlMCMGCCsGAQUFBzAChhdodHRwOi8veXIyLmkubGVuY3Iu
b3JnLzAnBgNVHREEIDAegg4qLm1hdXJ5Y3l6LmNvbYIMbWF1cnljeXouY29tMBMG
A1UdIAQMMAowCAYGZ4EMAQIBMC8GA1UdHwQoMCYwJKAioCCGHmh0dHA6Ly95cjIu
Yy5sZW5jci5vcmcvMTEwLmNybDCCAQsGCisGAQQB1nkCBAIEgfwEgfkA9wB1AExj
3JjlnB2riPYeij3ero+rRKM3e1+blMP7oZz8wb4mAAABocGRbw4AAAQDAEYwRAIg
HShWaEI2nVJEizSI6sJ/jnLaABZ3aTKoUL3pLo1uOgsCIEn+XiT2WHhm9LGN6kug
O9R7Z4q0HiFA4RZ7tIMJQH5XAH4AjspHC6zeavOiBrCkeoS3Rv4fxr+VPiXmm07k
AkjzxugAAAGhwZFyfQAIAAAFAEE0hdwEAwBHMEUCIDSms4Vf0llcvcldJd2Ool8i
AiUQQlS0Zzbej+7Y1aR7AiEA79Ll+t6inu1PkUIgzNTXkE0S20VHYEYel5fkdKSA
VTQwDQYJKoZIhvcNAQELBQADggEBAFi3A4fcjj4UKa9MFz1a8DWc+Jya2gJng13m
2UFdWCaqsN7FJWQEjRiJ0npynmDnA3VMyjswUMHAMx4d8G3lHNPfZWGsCgpjVa/d
IEoeZ6gTYjFa/Ovb64m4CcuQkm0GxcyAqMPs348e6Px32iU1R6MnV4UEUosnSPeK
4ocuI3SoR9LHxX/yHOKcYTKLpIMwMdaAmWt0N7l59GOb8o+dz3R7cxtVV/lrZReZ
/YPEAYRiNi7f+jNI1JCp0i8wXUb9NN7WlbmcFDecDG/++cRpAkoIk4m/LJindsuu
kErrVZ4yLqB0Hvm+uNvWxr9y/qKFTYw9POx1oaZJO/SwXZdDBeY=
-----END CERTIFICATE-----
```

*First, let's look at mathematical framework* of RSA. To generate a key, pick two large prime numbers (p and q) and multiply them to yeild the modulus (n). Pick a small number, e as public exponent.

The RSA public key is composed of the modulus (n) and the public exponent:

```
# Encryption requires knowing e (public)
cyphertext = plaintext^e mod n
```

The private key contains the modulus and the private exponent (d) which is chosen to be the multiplicate inverse of e modulo n. Exactly how d is found is not relevent, but the process requires knowing p and q.

```
# Decryption requires knowing d (pivate)
= cyphertext^d mod n

= plaintext^e^d mod n
= plaintext^(e*d) mod n
= plaintext^1 mod n
= plaintext
```

*Ok, back to vanity keys.* The standard value for e is 65537 (0x10001), because the small number of set bits makes encryption faster. In fact, the CA I used will refuse to sign keys with a non-standard public exponent, so I can't do anything funny there.

The modulus, n, is either 2,048 or 4,096 "user controlled" bits, so it's the ideal place to hide a message. However, to use the certificate, I'll need to find two large primes that multiply out to the vanity modulus.

My method starts by picking two random primes and calculating the resulting n value. Then, the bytes of n are modified to introduce message into the base64 encoding. Of course, the modified n is not the product of p and q, but this is fixable by setting q to n/p.

The resulting q value is very unlikely to be a prime (or integer), so I round it and find the next prime after the ideal q: which usually only requires changing the value of q by a few hundred. Since p and q are similar in size, a small change q only affects the least-sigifiant half of the product:

```
58174124 * 918491824 = 53432457262362176
(incrementing until the next prime...)
58174124 * 918491843 = 53432458367670532
```

As long as the message is embedded into the upper half of n, it's likely to survive undamaged. The original p and newly selected q can now be used to construct a functional RSA private key in the usual manner.

From here, it's really just a matter of getting a certificate aurthority to sign the key:

```
# Single domain
openssl req -new -key priv.pem -subj '/C=US/CN=example.com' > cert_request
certbot certonly --manual --csr cert_request

# Wildcard
cat > config<<EOF
[ req ]
req_extensions = req_ext

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = *.example.com
DNS.2 = example.com
EOF
openssl req -new -key priv.pem -config config -subj '/C=US/CN=example.com' > cert_request
certbot certonly --manual --csr cert_request
```

... although since base64 operates on groups of three byte, it is possible for the message to be misaligned and not readable. However, since the layout of the certificate is static, it is possible to adjust the message's placement until it works: this should take less than four attempts.

The result is a functional, trusted certificate with a longish message hidden in the public key. Renewal is done by resumbitting the CSR and repeating the ACME chalange, which can be automated.

Base64 is missing most punctuation, but it provides the full ASCII alphabet, and the ~150 (RSA-2048) or ~320 (RSA-4096) avalable charaters provides plenty of space for jokes. Another option would be to target the base16 displayed in most browser's certificate viewer: this would provide a lot more space (2 chars/byte instea of 0.75), but requires careful word choice.

*As for security*, this trick obviously reduces the entropy in the private key. However, it should still be secure: nearly identical methods have been considered as a form of compression. If the top-half of the modulus is fixed to a well known value, it doesn't have to be sent or stored as part of the key.

A reduction of around 1/2 can be achieved through division, but the state of the art can fix 2/3 of the key, reducing the size to 1/3 of standard RSA while retaining a similar level of security... although I should note that such tricks have largly been abandoned in favor of other crytosystems which can directly use small keys.

The manipulation of q may produce primes that are very close together, which makes it possible to factor n and recover the private key. A very small d value can also be a problem. Both of these are unlikely, but my script doesn't check for insecure values

However, regardless of the key's security, it should be perfectly fine for a public website: modern TLS provides forward secrecy, so eavesdropping requires a man-in-the-middle attack even if the private key is compromised.

## *Related*:

- [https://tom7.org/httpv/httpv.pdf](https://tom7.org/httpv/httpv.pdf): \[⭐ World's worst TLS server \]
- [/projects/badcert/gen.py](https://maurycyz.com/projects/badcert/gen.py): script to generate a vanity key
- [https://rya.nc/cert-tricks.html](https://rya.nc/cert-tricks.html): More vanity certs
- [https://doi.org/10.1007/978-3-540-79104-1_9](https://doi.org/10.1007/978-3-540-79104-1_9): Actual cryptographer's opinions on fixing part of n
