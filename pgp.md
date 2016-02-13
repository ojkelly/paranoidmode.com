# How Pretty Good Privacy (PGP) works

Learn more about your [root key](pgp/root_key.md) and how [subkeys work](pgp/subkeys.md).

PGP uses both asymmetric encryption and symmetric encryption. When encrypting a file (or message), the file is encrypted with a symmetric key, and the symmetric key is encrypted with asymmetric encryption.

## **Symmetric Encryption**

Alice has a wants to encrypt a file. She puts it in a box with a lock. The `key` to the lock is `secret`. She keeps a copy of this `key` to unlock the box later.

Alice now wants the file she encrypted. She unlocks (decrypts) the box with her `key`.

Pretty simple, one `key` both encrypts and decrypts our box. Or to put it another way both the lock and key are `secret`.

### Benefits and Threats

Symmetric encryption has it's benefits, but it also has threats that prevent it usage in all cases.

Symmetric encryption is fast, much faster than asymmetric encryption.

If Alice decides to send a box locked with the key `secret` to Bob, she also needs to tell Bob the key. It's possible to use a different channel to send the key. In all cases the key has a high risk of exfiltration (theft), rendering the encryption useless.

Symmetric encryption by itself is dangerous, in that it's risky. But when paired with asymmetric encryption, we can use the benefits of both. This is what PGP does.

## **Asymmetric Encryption / Key Pairs**

Asymmetric encryption is different in that you need one `key` to encrypt and a different `key` to decrypt. The way this works is by using a mathematical function that is easy in one direction and hard in the other.

We need the encryption function to be easy to do, and the decryption to be hard.

I could explain this, but Khan academy have done it so well, you need to watch this first.

{% youtube %}https://www.youtube.com/watch?v=EPXilYOa71c{% endyoutube %}

There's a heap of videos in that course, [that are all worth watching](https://www.khanacademy.org/computing/computer-science/cryptography).

### PGP Key pairs

Your PGP key pair consists of two parts: the `private key` and the `public key`.

It's vital that you never lose ultimate control of your `private key`. This means you need to take every (sometimes excessive) precaution to ensure your `private key` is not stolen, copied, or transmitted. If this happens, you need to revoke your `private key` and generate a new one as soon as possible.

#### Private Key

Your `private key` is your `public key` plus the `secret information` that you need to make the hard problem of decryption, into an easy problem.


#### Public Key

Your `public key` is best thought of as a schematic for a lock. It describes precisely how to build the lock. This is a special lock, due to it's complexity knowing how to build it, and use the lock function, doesn't mean you know how to unlock it.

It is possible to unlock the lock through a process called factorization. But with traditional computers and when you're using large key sizes, it's technically not feasible.




## Threats to PGP

You should be aware of of [Shor's Algorithm](https://en.wikipedia.org/wiki/Shor%27s_algorithm). It desrcibes a method that will break public key encryption very quickly. Effectively making the hard problem (decryption) and easy as the easy problem (encryption).

Naturally the NSA (and presumably other <acronym title="Three Letter Agencies">TLA</acronym>  are [working on being able to do this](https://www.washingtonpost.com/world/national-security/nsa-seeks-to-build-quantum-computer-that-could-crack-most-types-of-encryption/2014/01/02/8fff297e-7195-11e3-8def-a33011492df2_story.html). If this does become possible we will likely hear about it 5-10 years after it's discovered. If we're lucky we'll find out quickly.

The NSA also reporterdly records every PGP message they see, in the hope they can decrypt them later. Is they can solve Shor's Algorithm then they will be able to do this.
