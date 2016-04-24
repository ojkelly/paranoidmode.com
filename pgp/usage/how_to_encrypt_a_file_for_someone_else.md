# How to encrypt a file for someone else

Let's say you're going to send me a file.

First you need my public key, so let's go and get that.

    gpg2 --recv-keys 2F95015B
    
> Where did `2F95015B` come from? It's the short key of my public key. You will need the find the appropriate one for the person you wish to encrypt for. This is not a small topic, so first read [how to find someone's public key](usuage/how_to_find_someones_public_key.md).

You should see something like:

    gpg: requesting key 2F95015B from hkp server pgp.mit.edu
    gpg: key 2F95015B: public key "Owen Kelly <owen@owenkelly.com.au>" imported
    gpg: Total number processed: 1
    gpg:               imported: 1  (RSA: 1)

If you don't you might not have the right key.

Now you have the key in your keyring, you can encrypt the file `secret.md` as follows:

    gpg2 --encrypt secret.ms --out output.asc --recipient "Owen Kelly"
