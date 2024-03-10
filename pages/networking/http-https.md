---
title: "HTTP/HTTPS"
---

# HTTP/HTTPS

![](https://www.cloudflare.com/img/learning/security/glossary/what-is-ssl/http-vs-https.svg)


HTTPS is HTTP with encryption and verification. The only difference between the two protocols is that HTTPS uses TLS (SSL) to encrypt normal HTTP requests and responses, and to digitally sign those requests and responses. As a result, HTTPS is far more secure than HTTP. A website that uses HTTP has http:// in its URL, while a website that uses HTTPS has https://.

## What is HTTP?

HTTP stands for Hypertext Transfer Protocol, and it is a protocol – or a prescribed order and syntax for presenting information – used for transferring data over a network. Most information that is sent over the Internet, including website content and API calls, uses the HTTP protocol. There are two main kinds of HTTP messages: requests and responses.

In the OSI model, HTTP is a layer 7 protocol.

If a website uses HTTP instead of HTTPS, all requests and responses can be read by anyone who is monitoring the session. Essentially, a malicious actor can just read the text in the request or the response and know exactly what information someone is asking for, sending, or receiving.

## What is HTTPS?

The S in HTTPS stands for "secure." HTTPS uses TLS (or SSL) to encrypt HTTP requests and responses, so in the example above, instead of the text, an attacker would see a bunch of seemingly random characters.

Instead of:

```
GET /hello.txt HTTP/1.1
User-Agent: curl/7.63.0 libcurl/7.63.0 OpenSSL/1.1.l zlib/1.2.11
Host: www.example.com
Accept-Language: en
```

The attacker sees something like:
```
t8Fw6T8UV81pQfyhDkhebbz7+oiwldr1j2gHBB3L3RFTRsQCpaSnSBZ78Vme+DpDVJPvZdZUZHpzbbcqmSW1+3xXGsERHg9YDmpYk0VVDiRvw1H5miNieJeJ/FNUjgH0BmVRWII6+T4MnDwmCMZUI/orxP3HGwYCSIvyzS3MpmmSe4iaWKCOHQ==
```

## In HTTPS, how does TLS/SSL encrypt HTTP requests and responses?

TLS uses a technology called public key cryptography: there are two keys, a public key and a private key, and the public key is shared with client devices via the server's SSL certificate. When a client opens a connection with a server, the two devices use the public and private key to agree on new keys, called session keys, to encrypt further communications between them.

All HTTP requests and responses are then encrypted with these session keys, so that anyone who intercepts communications can only see a random string of characters, not the plaintext.

## How does HTTPS help authenticate web servers?

Authentication means verifying that a person or machine is who they claim to be. In HTTP, there is no verification of identity – it's based on a principle of trust. The architects of HTTP didn't necessarily make a decision to implicitly trust all web servers; they simply had priorities other than security at the time. But on the modern Internet, authentication is essential.

Just like an ID card confirms a person's identity, a private key confirms server identity. When a client opens a channel with an origin server (e.g. when a user navigates to a website), possession of the private key that matches with the public key in a website's SSL certificate proves that the server is actually the legitimate host of the website. This prevents or helps block a number of attacks that are possible when there is no authentication, such as:

+ On-path attacks
+ DNS hijacking
+ BGP hijacking
+ Domain spoofing

In addition, the SSL certificate is digitally signed by the certificate authority that issued it. This provides confirmation that the server is who it claims to be.

## What is encryption?

## How does encryption work?

Encryption is a mathematical process that alters data using an encryption algorithm and a key. Imagine if Alice sends the message "Hello" to Bob, but she replaces each letter in her message with the letter that comes two places later in the alphabet. Instead of "Hello," her message now reads "Jgnnq." Fortunately, Bob knows that the key is "2" and can decrypt her message back to "Hello."

Alice used an extremely simple encryption algorithm to encode her message to Bob. More complicated encryption algorithms can further scramble the message:

![](https://cf-assets.www.cloudflare.com/slt3lc6tev37/4zLJngHjth92rb9VUrclZr/ec5b406b06e1fbee7dc0d5950789ce76/encryption-example.svg)

Although encrypted data appears random, encryption proceeds in a logical, predictable way, allowing a party that receives the encrypted data and possesses the right key to decrypt the data, turning it back into plaintext. Truly secure encryption will use keys complex enough that a third party is highly unlikely to decrypt or break the ciphertext by brute force — in other words, by guessing the key. (Alice's first encryption method would be broken very quickly.)

Data can be encrypted "at rest," when it is stored, or "in transit," while it is being transmitted somewhere else.

## What is a key in cryptography?

A cryptographic key is a string of characters used within an encryption algorithm for altering data so that it appears random. Like a physical key, it locks (encrypts) data so that only someone with the right key can unlock (decrypt) it.:

## What are the different types of encryption?
The two main kinds of encryption are symmetric encryption and asymmetric encryption. Asymmetric encryption is also known as public key encryption.

In symmetric encryption, there is only one key, and all communicating parties use the same (secret) key for both encryption and decryption. In asymmetric, or public key, encryption, there are two keys: one key is used for encryption, and a different key is used for decryption. The decryption key is kept private (hence the "private key" name), while the encryption key is shared publicly, for anyone to use (hence the "public key" name). Asymmetric encryption is a foundational technology for TLS (often called SSL).

## Why is encryption important?
+ **Privacy**: Encryption ensures that no one can read communications or data at rest except the intended recipient or the rightful data owner. This prevents attackers, ad networks, Internet service providers, and in some cases governments from intercepting and reading sensitive data, protecting user privacy.

+ **Security**: Encryption helps prevent data breaches, whether the data is in transit or at rest. If a corporate device is lost or stolen and its hard drive is properly encrypted, the data on that device will still be secure. Similarly, encrypted communications enable the communicating parties to exchange sensitive data without leaking the data.

+ **Data integrity**: Encryption also helps prevent malicious behavior such as on-path attacks. When data is transmitted across the Internet, encryption ensures that what the recipient receives has not been viewed or tampered with on the way.

+ **Regulations**: For all these reasons, many industry and government regulations require companies that handle user data to keep that data encrypted. Examples of regulatory and compliance standards that require encryption include HIPAA, PCI-DSS, and the GDPR.

## What is an encryption algorithm?
An encryption algorithm is the method used to transform data into ciphertext. An algorithm will use the encryption key in order to alter the data in a predictable way, so that even though the encrypted data will appear random, it can be turned back into plaintext by using the decryption key.:

## What are some common encryption algorithms?
Commonly used symmetric encryption algorithms include:

+ AES
+ 3-DES
+ SNOW

Commonly used asymmetric encryption algorithms include:

+ RSA
+ Elliptic curve cryptography

## What is a brute force attack in encryption?

A brute force attack is when an attacker who does not know the decryption key attempts to determine the key by making millions or billions of guesses. Brute force attacks are much faster with modern computers, which is why encryption has to be extremely strong and complex. Most modern encryption methods, coupled with high-quality passwords, are resistant to brute force attacks, although they may become vulnerable to such attacks in the future as computers become more and more powerful. Weak passwords are still susceptible to brute force attacks.

## How is encryption used to keep Internet browsing secure?

Encryption is foundational for a variety of technologies, but it is especially important for keeping HTTP requests and responses secure. The protocol responsible for this is called HTTPS (Hypertext Transfer Protocol Secure). A website served over HTTPS instead of HTTP will have a URL that begins with https:// instead of http://, usually represented by a secured lock in the address bar.

HTTPS uses the encryption protocol called Transport Layer Security (TLS). In the past, an earlier encryption protocol called Secure Sockets Layer (SSL) was the standard, but TLS has replaced SSL. A website that implements HTTPS will have a TLS certificate installed on its origin server.
