# Key pair





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


