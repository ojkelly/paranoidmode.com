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

    # With the name associated with the public key
    gpg2 --out output.asc --recipient "Owen Kelly" --encrypt secret.md
  
    # With the email from the public key
    gpg2 --out output.asc --recipient owen@owenkelly.com.au --encrypt secret.md
    
    # With the short key
    gpg2 --out output.asc --recipient 2F95015B --encrypt secret.md 

With method you choose in up to you.

The output filename can be whatever you want. It's conventional to end in `.asc`, but you can put whatever you want. It's likely not a good idea to call it something like `secret.md.asc`.