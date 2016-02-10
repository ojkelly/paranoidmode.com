# How Pretty Good Privacy (PGP) works

Learn more about your [root key](pgp/root_key.md) and how [subkeys work](pgp/subkeys.md).


Key pairs are a practical application of asymmetric encryption. 

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




## **More information**

{% youtube %}https://www.youtube.com/watch?v=MsqqpO9R5Hc{% endyoutube %}

I'd also strongly reccommend watching the entire [Khan Academy course on cryptography](https://www.khanacademy.org/computing/computer-science/cryptography).
