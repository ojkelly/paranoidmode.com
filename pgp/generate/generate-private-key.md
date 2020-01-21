# Generate your private key

Open up Terminal on Tails and type `sudo gpg2 --gen-key`.

> We're using `sudo` here, as we need to use `sudo` to interact with the `OpenPGP Smart Card` and the
 gpg keyring is seperate for `root`.

    amnesia@amnesia:/~$ sudo gpg2 --gen-key

Tails will lecture you about using `sudo`. Read it and understand *here be dragons*.

    We trust you have received the usual lecture from the local System
    Administrator. It usually boils down to these three things:

        #1) Respect the privacy of others.
        #2) Think before you type.
        #3) With great power comes great responsibility.

Enter in the `root` password you set when you configured the pre-launch wizard for Tails
 (you'll need this password quite a lot).

    [sudo] password for amnesia:
    gpg (GnuPG) 2.0.25; Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    gpg: keyring `/root/.gnupg/pubring.gpg' created

Your new keyring has just been created (this is where PGP stores its keys). Now we need
to choose what type of key we are generating.
Because this is going to be a *long term offline* `private key`, and we are going to use
 subkeys for day to day use, we want to select `4 RSA (sign only)`.

    Please select what kind of key you want:
       (1) RSA and RSA (default)
       (2) DSA and Elgamal
       (3) DSA (sign only)
       (4) RSA (sign only)
    Your selection? 4

Now you get to choose the length of your `private key`, you need to select **at least** `4096`.

 If for some reason I haven't updated this and the accepted wisdom is to use higher than 4096, do that.
 The only cost of using a larger key is it takes a bit longer to use. However smaller keys are
 almost exponentially quicker to brute force. So as computing power increases we increase
  our key sizes to ensure they cannot be brute forced.

    RSA keys may be between 1024 and 4096 bits long.
    What keysize do you want? (2048) 4096
    Requested keysize is 4096 bits

Next up how long should this key last for?

Well ideally it's your *long term offline* `private key`, so it should never expire. If you remain a good steward of this
key, you should be able to use it for the extent of your natural life.

Set this to `0` so it does not expire, and then type `y` at the next prompt to confirm your choice.

    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 0
    Key does not expire at all
    Is this correct? (y/N) y

Now we get to add the `user ID` to your key. This is the primary way other than the `fingerprint` that your key can be
 identified, it's commonly used when searching for a key. Set this to your primary email. If this is a personal key, use a
 personal email. First though enter your full name.

> **If you need to ensure this key cannot be connected to you**, make sure you never enter any identifiable information here,
and make something up.

    GnuPG needs to construct a user ID to identify your key.

    Real name: Owen Kelly
    Email address: owen@owenkelly.com.au
    Comment:
    You selected this USER-ID:
        "Owen Kelly <owen@owenkelly.com.au>"

Then type the letter `O` to confirm is the above is correct, or you can change the information if you entered it wrong.

    Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

Remember that `diceware` passphrase you created?

 You'll see a popup outside of terminal. Enter your `diceware` passphrase here. This encrypts your private key with your
 `diceware` passphrase. You will need to enter this passphrase everytime you need to decrypt the private key
  to perform an operation (that is, sign something).

    You need a Passphrase to protect your secret key.


Now we're about to actually start generating the key. As the message says, the computer needs to capture as much
entropy as possible. Think of it as similar to randomness, it's what makes the key less predictable.
A predictable key is a crackable key.

Basically, move your mouse and type on the keyboard (don't hit `ctrl-c` though, as you'll cancel the key generation).

    We need to generate a lot of random bytes. It is a good idea to
    perform some other action (type on the keyboard, move the mouse,
    utilize the disks) during the prime generation; this gives the
    random number generator a better chance to gain enough entropy.
    gpg: key 2F95015B marked as ultimately trusted
    public and secret key created and signed.

The key has been generated!

Now `gpg` will automatically trust the key, and report back information about the key.
This information is public, thus okay to share. The key you see below is my actual keypair.

    gpg: checking the trustdb
    gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
    pub   4096R/2F95015B 2016-01-19
          Key fingerprint = 24E7 19D9 E009 27E9 C5CF  1423 A002 84BB 2F95 015B
    uid       [ultimate] Owen Kelly <owen@owenkelly.com.au>

    Note that this key cannot be used for encryption.  You may want to use
    the command "--edit-key" to generate a subkey for this purpose.

Note in the last message above that the key cannot be used for encryption. This is because we chose only to allow our
`private key` to sign other keys. This is the perfect model for using subkeys.

Take note of the line `pub   4096R/2F95015B 2016-01-19`.

`pub` refers to this being your `public key`.

After the `4096R/` is your key's `fingerprint`. In this example my `fingerprint` for my `public key` is `2F95015B`.
 Note this down or remember it. It's *public* information (so no need to keep it secret) and it serves as a quick ID for
 the key. You'll need to use it in a bit when working with this key.

