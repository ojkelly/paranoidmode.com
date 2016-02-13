# Key pair

Key pairs are a practical application of asymmetric encryption. 

## **Symmetric Encryption**

Alice has a wants to encrypt a file. She puts it in a box with a lock. The `key` to the lock is `secret`. She keeps a copy of this `key` to unlock the box later.

Alice now wants the file she encrypted. She unlocks (decrypts) the box with her `key`.

Pretty simple, one `key` both encrypts and decrypts our box. Or to put it another way both the lock and key are `secret`.

### Benefits and Threats

Symmetric encryption has it's benefits, but it also has threats that prevent it usage in all cases.

Symmetric encryption is fast, much faster than asymmetric encryption.

If Alice decides to send a box locked with the key `secret` to Bob, she also needs to tell Bob the key. It's possible to use a different channel to transmit the key. In all cases the key has a high risk of being exfiltrated, rendering the encryption useless.

Symmetric encryption by itself is dangerous, in that it's risky. However 


{% youtube %}https://www.youtube.com/watch?v=MsqqpO9R5Hc{% endyoutube %}

I'd also strongly reccommend watching the entire [Khan Academy course on cryptography](https://www.khanacademy.org/computing/computer-science/cryptography).

## **Detailed Explanation**



We need the encryption function to be easy to do, and the decryption to be hard.





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


