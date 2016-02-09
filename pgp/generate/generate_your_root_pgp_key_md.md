# Generate your root PGP key

### **What you will be able to do**

After completing this guide you will be able to:

 - receive and `decrypt` files and emails encrpyted with your `public key`,
 - `sign` files, text and other peoples public keys
 - `authenticate` yourself using PGP


----

### **What you will need**

Before you get started you will need to get the following:

 - **1-2 hours** (I think)
 - `Dice`
   - ideally you want 8, but 1 will do
 - `GPG Tools`
   - [Download GPG Tools](https://www.gpgtools.org/)
 - **1** `Tails.iso`
   - [Download Tails](https://tails.boum.org/)
 - **3** brand new `USB drives`.
   - 1 for our Tails installation called `TAILS`,
   - 1 to store your `private key` and `revocation certificate` called `PRIVATE`,
   - 1 to transfer your `public key` your normal computer called `PUBLIC`,
 - **1 Yubikey 4** or newer, or similar `OpenPGP Smart Card`
   - I'm using a Yubikey because they're small, well built, and easy
   - The Yubikey 4 allows for 4096 bit keys, previous Yubikey's do not allow keys this big, and some older
      Yubikeys don't support `OpenPGP Smart Card`. So make it easy for yourself, get a new Yubikey 4 or newer.
 - **1** `air-gapped laptop`
   - Ideally this is a laptop with the following removed or not installed: `microphone`, `speakers`,
   `webcam/camera`, `wifi antenna`, `bluetooth antenna`, `3G/4G/5G/6G/7G antenna`, `hard drives`, and any other devices capable of transmitting information wirelessly.
   - It is possible to use your normal computer and boot Tails on it, but I wouldn't recommend it. Best to be paranoid here.
 - **1** `everyday computer`
   - This is the one you normally use, and will use day to day
 - **1** `non-wireless printer`
   - You'll need one that works with Tails, check to see if there are Linux drivers available,
   - You can technically use a wireless capable printer, but as we are going to use this to print out a backup,
    of your `private key`, using a wireless capable, or internet connected computer adds significant risk that your `private key` will be exfiltrated and rendered insecure.
 - A safe place to store a backup of your private key, common locations are:
   - a `safe` (the metal kind with a code),
   - your parents house, or the house of someone you trust,
   - any location where you can be sure you will know if it has been accessed.

You might also want to get some tamper evident bags to store your `private key` backups inside of. I'll leave this up to you.


----

### **Assumed Knowledge**

Writing a guide like this, it's very hard to cater to all knowledge levels. It's hard enough
just to remember exactly how to setup your keys. So this guide has been written with the following assumptions:

  - You are using Linux, Unix or OS X
  - You can use terminal
     - You don't need to have mastered it, but you need to know what it is, and how to open it up

----

### **Some things you might not know**

  - `crtl-c` will cancel a running command in terminal
    - useful when something gets stuck, you hit the wrong command or you need to stop something
  - `up` and `down` let you cycle recent commands
  - In `Tails` and other Linux terminals to copy text from the terminal you use `Shift-Ctrl-C` and `Shit-Ctrl-V`
  - In `Tails` you need to be root to access an OpenPGP smartcard (that's the Yubikey)

You should also know that there are known malware programs who's only job is to scan everything it can looking for `private keys`. These keys are super obvious as they always start with:

      -----BEGIN PGP PRIVATE KEY BLOCK-----

So yes, be paranoid about your private key touching any networked computer, it will take barely a second for the key to be exfiltrated, and rendered insecure.

### PGP and GPG

> Pretty Good Privacy (PGP) is a data encryption and decryption computer program that provides cryptographic
  privacy and authentication for data communication. PGP is often used for signing, encrypting, and decrypting
  texts, e-mails, files, directories, and whole disk partitions and to increase the security of e-mail
  communications. It was created by Phil Zimmermann in 1991. <br>
  <br>
  [Wikipedia](https://en.wikipedia.org/Pretty_Good_Privacy)

**Pretty Good Privacy (PGP)** has since been defined as a standard called OpenPGP. It's outlined in [RFC4880](https://tools.ietf.org/html/rfc4880).
[Requests For Comment](https://en.wikipedia.org/wiki/Request_for_Comments) at the IETF are standards documents even if the name doesn't immediately suggest it.

**GNU Privacy Guard (GPG)** also GnuPG. GPG is an implementation of OpenPGP and can be found on Linux, OS X and Windows.

 With PGP and GPG often they're used interchangeably. Don't be too concerned if you get them mixed up.
 When referring to PGP or GPG keys, everyone (including you now) knows you mean a public/private keypair.

----

### **What you will have after this**

Once you've completed this guide, you will have the following:

  - 1 `diceware` passphrase
    - likely written down and in your wallet. Burn when you can remember it.
  - 1 USB with the following stored on it:
    - your `private key` which is only capable of `certification`
    - a `revocation certificate` for your `public key`
    - **This USB is never allowed to be connected to a computer that is connected to a network. It should only ever connect to an air-gapped Tails computer**
  - 1 USB with the following stored on it:
    - your `public key`
  - 1 Yubikey (or other OpenPGP Card) with the following stored on it:
    - 1 `subkey` for `encryption`
    - 1 `subkey` for `signing`
    - 1 `subkey` for `authentication`
  - 1 `paperkey` a printed out backup of the secret parts of your `private key` that is to be used if the USB with your `private key` stops working.
  - Your public key will be distributed to the public key servers
  - Keybase currently has a bug preventing it from working with Yubikeys, so this is not yet covered - Jan 2016


----

### **Table of Contents**

1. [Generate your `diceware` passphrase](#diceware)
2. [Prepare your Yubikey](#prepare-yubikey)
3. [Prepare your USBs](#prepare-usbs)
4. [Boot Tails on your air-gapped laptop](#tails)
5. [Generate your private key](#generate-private-key)
6. [Generate revocation certificate](#generate-revocation-certificate)
7. [Backup your private key](#backup-private-key)
8. [Generate your subkeys](#generate-subkeys)
9. [Publish your public key](#publish-public-key)
10. [Done](#next-steps)

<a name="diceware"></a>

----

## 1. Generate your `diceware` passphrase

Passphrase? Don't you mean password?<br>
**No.**


A passphrase is a different to a password. Most people are terrible at making passwords. They're always too short,
and often are phrases commonly found in common use. Like, `to2ornot2b`. Literally the worst password after `12345`,
`qwerty` and well a few others like `dog1992`.

Passwords get their strength from [entropy](https://www.wikiwand.com/en/Entropy_(computing)), it's best thought of as randomness, or unpredictability. Encryption is easier and faster to crack when you can somewhat predict what the key might be.

I present one of the most famous XKCD's for your entertainment.

[![XKCD ](password_strength.png)](https://xkcd.com/936/)

This XKCD is a little misleading though, a good diceware passphrase needs more than 4 words. My current recommendation is to use `8 words` or more. If you can remember 10 words, use 10.

Here's what a 10 word passphrase looks like

    witt splay ash fried arena ryan apathy chief evoke kane




###  Let's make a diceware passphrase

> There are websites that will generate diceware passphrases for you. There is only **one** way to make a diceware passphrase worth using. Dice.
> The problem with letting a computer generate it for you, is that is lacks the amount of entropy you can get with a dice and is significantly less secure as a result.

Download and print out [this Diceware list](/public/files/dicewarewordlist.pdf) - it's 37 pages.<br>
You can also get this list from the [Diceware hompage](http://world.std.com/~reinhold/diceware.html).<br>
Or get a version [signed by the creator](http://world.std.com/~reinhold/diceware.wordlist.asc).<br>

So you now have 37 printed pages which is where you will source a passphrase from. If you decide not to print out the list, **do not search through the page to find your words**. Scroll through the page and scan with your eyes.

Take your dice and roll 5 (or 1 dice 5 times) for every word you are going to generate.

For 8 words you'll roll 40 dice, for 10 words roll 50.

Find a nice spot where you can give your dice a good roll, something more than half a meter. I roll them on the floor.

Starting from the top write the numbers down on a new clean sheet of paper. The dice will not distribute evenly or in a neat line, so do your best to scan left to right starting from the top. Or any way really, just don't double up.

    64312552461253126346124315246512345164512465135156

Once you have all your numbers, you'll likely find it easy to group the numbers into clusters of 5.

    (64312)(55246)(12531)(26346)(12431)(52465)(12345)(16451)(24651)(35156)

After that translate the 5 digit numbers into their diceware word.

    (64312) = witt
    (55246) = splay
    (12531) = ash
    (26346) = fried
    (12431) = arena
    (52465) = ryan
    (12345) = apathy
    (16451) = chief
    (24651) = evoke
    (35156) = kane

Some people like to come up with a story to help them remember. This isn't my passphrase, and I'm not very good at the whole make a story thing, so enjoy this:

> **Witt** **splay**ed the **ash** while **fried** played in the **arena** with **ryan** who had **apathy** to the **chief**. Time to **evoke** **kane**.

The point with diceware passphrases are they they aren't common groupings of words, and the result isn't something that
follows normal speech. As that would be predictable and less secure. So your passphrase story is likely
just a manner to make the keywords a bit less nonsensical so you can remember them.

You should now have a sheet of paper with 40 or 50 numbers on it, and your diceware passphrase below it.
Keep this paper very safe. Fold it up and put it in your wallet. You will need to refer to it for a
couple of days until you memorize the password. After that you might want to store it with your `private key` backups.
If you forget this passphrase you will loose access to your private key. If you loose this passphrase, you can change it.


<a name="prepare-yubikey"></a>

----


## 2. Prepare your Yubikey

We don't need to do much here, but we do need to ensure the
Yubikey you have can be used to store PGP keys, and that you have access to it.

Change Pin and Admin codes

<a name="prepare-usbs"></a>

----

## 3. Prepare your USBs

This is all mostly straightforward. We're going to format 2 USB's and label them. Then we're going to install Tails on the 3rd one.

### PRIVATE


### PUBLIC


### TAILS


<a name="tails"></a>

----

## 4. Boot Tails on your air-gapped laptop

> **STOP AND READ THIS BEFORE CONTINUING**
>
> Before booting into Tails ensure that there are no wireless transmitters in the laptop (wifi, bluetooth, cellular).
>
> Ask make sure there is **no hard drive** or **storage media** excluding your USBs connected or installed in the hard drive.
>
> Remember we're working with your private key on this laptop, as such we must take every precaution to
ensure it cannot be exfiltrated.


**Did you read the big message above? You did? So there's absolutely
no wireless radios or data storage media other than you're USBs?**

If you cheap out here you might be putting yourself at risk.
You should be able to get a super crappy laptop for $150 or less off ebay.
You barely need anything to run Tails. So get a dedicated laptop for airgapped work.

Okay let's move on.

Plug your Tails usb into your laptop and boot from the USB. You might
 need to toggle the boot order in the BIOS to set the USB drive as the first boot media.

When you boot Tails you're greeted with a pre-launch wizard that gives you some options. The only thing you need
to do is set an `admin password`. There are other options you can set but as this device never touches a
network you can just set the admin password and launch Tails.

You need to admin password set so that you can use `sudo` to interact with the `OpenPGP Smart Card`. The
drivers don't allow you to interact if you're not `root` (which is what `sudo` does).

A quick and easy way to mount a USB drive with Tails is to open it with the <abbr title="Graphical User Interface">GUI</abbr>.


> TODO Add instructions regarding printing, and checking that you can print.

### Prepare your USBs

Grab your two left over USBs. Designate one as your `private USB` and the other `public USB`. If they are physically different
this should be easy, otherwise mark one to indicate the difference.

It's handy to format these two drive to be labeled `PRIVATE` and `PUBLIC`. That way you won't get mixed up moving files around.

**You need to ensure your `private USB` never connects to anything other than your airgapped laptop.**

Now to mount your `private USB` and `public USB`.

<a name="generate-private-key"></a>

----

## 5. Generate your private key

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

Your new keyring has just been created (this is where PGP stores it's keys). Now we need
to choose what type of key we are generating.
Because this is going to be a *long term offline* `private key`, and we are going to use
 subkeys for day to day use, we want to select `4 RSA (sign only).

    Please select what kind of key you want:
       (1) RSA and RSA (default)
       (2) DSA and Elgamal
       (3) DSA (sign only)
       (4) RSA (sign only)
    Your selection? 4

Now you get to choose the length of you're `private key`, you need to select **at least** `4096`.

 If for some reason I haven't updated this and the accepted wisdom is to use higher than 4096, do that.
 The only cost of using a larger key is it takes a bit longer to use. However smaller keys are
 almost exponentially quicker to brute force. So as computing power increase we increase
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

Basically, move your mouse and type on the keyboard (don't hit `ctrl-c` though you'll cancel the key generation).

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

Not the last message above, that the key cannot be used for encryption. This is because we chose only to allow our
`private key` to sign other keys. This is the perfect model for using subkeys.

Take note of the line `pub   4096R/2F95015B 2016-01-19`.

`pub` refers to this being your `public key`.

After the `4096R/` is your key's `fingerprint`. In this example my `fingerprint` for my `public key` is `2F95015B`.
 Note this down or remember it. It's *public* information (so no need to keep it secret) and it serves as a quick ID for
 the key. You'll need to use it a bit when working with this key.


<a name="generate-revocation-certificate"></a>

----

## 6. Generate revocation certificate

A revocation certificate is used when you can no longer use your `private key` and need to create a new one. It's a
certificate signed with your `private key` that says the key should no longer be used or trusted.

It's best practice to generate this certificate at the same time as `private key` generation. If your key is compromised,
or your `private USB` and `paperkey` backups no longer work you will need to transition to a new `private key`.

To generate your `revocation certificate` type `sudo gpg2 --gen-revoke 2F95015B > 2F95015B-revocation-certificate.asc`
 where `2F95015B` is the `fingerprint` of your key.

    amnesia@amnesia:/~$ sudo gpg2 --gen-revoke 2F95015B > 2F95015B-revocation-certificate.asc
    [sudo] password for amnesia:

GPG will then tell you the metadata of this key. Confirm it's correct before continuing.

    sec  4096R/2F95015B 2016-01-19 Owen Kelly <owen@owenkelly.com.au>

    Create a revocation certificate for this key? (y/N) y
    Please select the reason for the revocation:
      0 = No reason specified
      1 = Key has been compromised
      2 = Key is superseded
      3 = Key is no longer used
      Q = Cancel
    (Probably you want to select 1 here)
    Your decision? 1

GPG will recommend choosing `1` here, and you should. You don't know the circumstances when you will need this key.
     However you will only need this `revocation certificate` if you can no longer use your `private key` (you would just generate
     a more appropriate certificate if you had access).

Choose `1`.

Then enter a description similar to what follows here. I think it's informative to include that this `revocation certificate` was
generated at the same time as key generation.

    Enter an optional description; end it with an empty line:
    > This is a revocation certificate created at key generation, likely the private key has been compromised. Please obtain a new public key from me directly.
    >
    Reason for revocation: Key has been compromised
    This is a revocation certificate created at key generation, likely the private key has been compromised. Please obtain a new public key from me directly.
    Is this okay? (y/N) y

You will then need to enter your `diceware` passphrase to sign the certificate.

    You need a passphrase to unlock the secret key for
    user: "Owen Kelly <owen@owenkelly.com.au>"
    4096-bit RSA key, ID 2F95015B, created 2016-01-19

Once that's done you should have a `revocation certificate` file in your current directory.

    amnesia@amnesia:/~$ ls
    2F95015B-revocation-certificate.asc

Move it to your `private USB`. I called it `PRIVATE` so it's mounted at `/media/PRIVATE`, it might be different on your
setup.

    amnesia@amnesia:/~$ ls
    2F95015B-revocation-certificate.asc
    amnesia@amnesia:/~$ cp 2F95015B-revoke.asc /media/PRIVATE/

Check that it did in fact copy over with `ls /media/PRIVATE`.
If you see it listed in the output it's worked.

    amnesia@amnesia:/~$ ls /media/PRIVATE
    2F95015B-revoke.asc



<a name="backup-private-key"></a>

----

## 7. Backup your private key

We're going to export your `private key` and store it on your `private USB` and then we're going to make a paper backup.

### Private USB

Change directory `cd` into your `private USB`, most likely located at `/media/PRIVATE`.

Then run `sudo gpg2 --export-secret-key -a 2F95015B  >>  2F95015B-private.asc` replacing `2F95015B` in the 2 spots, with
your own `fingerprint`.

     amnesia@amnesia:/~$ cd /media/PRIVATE
     amnesia@amnesia:/media/PRIVATE$ sudo gpg2 --export-secret-key -a 2F95015B  >>  2F95015B-private.asc

List `ls` the contents of the directory to ensure it's exported it. Your `revocation certificate` should be there
as well.

     amnesia@amnesia:~/media/PRIVATE$ ls
     2F95015B-private.asc  2F9501B-revoke.asc

### Paperkey

Generating the `paperkey` is a similar process. We use a little unix feature `|` the pipe operater, to pipe the output of
the first function into the second function, `gpg2` into `paperkey` in this case, then we use `>>` to save the output of `paperkey`
to a file called `2F95015B-paperkey.txt`, make sure to substitute your fingerprint.

     amnesia@amnesia:/media/PRIVATE$ sudo gpg2 --export-secret-key 2F95015B | paperkey >>  2F95015B-paperkey.txt
     [sudo] password for amnesia:

After that's made, you can `ls` the directory to ensure it's been created. Now you'll need to print the `2F95015B-paperkey.txt`
 file. Open up the GUI file explorer, locate the file, and open it with `gedit`. Then print it.

     amnesia@amnesia:/media/PRIVATE$ ls
     2F95015B-paperkey.txt  2F95015B-private.asc   2F9501B-revoke.asc

Store your printed copy in a plastic envelope, as it's likely a few pages.

While you're here you can take the option to print out your `revocation certificate` and if you want, the `ascii amoured` version
 of your `private key` too. As these are in `ascii` you will need to manually type them up in order to use them.


<a name="generate-subkeys"></a>

----

## 8. Generate your subkeys

It's now time top plug in that `yubikey` or other `OpenPGP Smart Card`.

Make sure you know what the `pin` and `admin pin` are.

Firstly change back to your home directory by entering `cd ~`.

Then run `sudo gpg2 --card-status` to check if you're `smart card` is working. You should see an output similar to below.

    amnesia@amnesia:/~$ sudo gpg2 --card-status
    [sudo] password for amnesia:
    Application ID ...: D00000000000000000000000000000000
    Version ..........: 2.1
    Manufacturer .....: unknown
    Serial number ....: 00000000
    Name of cardholder: [not set]
    Language prefs ...: [not set]
    Sex ..............: [not set]
    URL of public key : [not set]
    Login data .......: [not set]
    Signature PIN ....: not forced
    Key attributes ...: 4096R 4096R 4096R
    Max. PIN lengths .: 127 127 127
    PIN retry counter : 3 0 3
    Signature counter : 2
    Signature key ....: [none]
    Encryption key....: [none]
    Authentication key: [none]
    General key info..: [none]
    amnesia@amnesia:/~$ scdaemon[6966]: updating slot 0 status: 0x0000->0x0007 (0->1)
    scdaemon[6966]: scdaemon (GnuPG) 2.0.25 stopped
    ^C

You might need to hit `ctrl-c` to escape from `--card-status`. That's what the `^C` at the end of the snippet above is.

Now we know you're `smart card` is working we can generate the `subkeys`.

Enter `sudo gpg2 --edit-key 2F95015B` with your fingerprint, to begin editing your `private key`.

    amnesia@amnesia:/~$ sudo gpg2 --edit-key 2F95015B

You'll need the root password here.

    [sudo] password for amnesia:

GPG will then let you know the key's metadata, and should inform you that the secret key is available.

    gpg (GnuPG) 2.0.25; Copyright (C) 2013 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Secret key is available.

    pub  4096R/2F95015B  created: 2016-01-19  expires: never       usage: SC
                         trust: ultimate      validity: ultimate
    [ultimate] (1). Owen Kelly <owen@owenkelly.com.au>

Type `addcardkey` to get started creatig the first `smart card` key pair.
We're going to add a `Signature key` first, so enter `1`.

    gpg> addcardkey
    Signature key ....: [not set]
    Encryption key....: [not set]
    Authentication key: [not set]

    Please select the type of key to generate:
       (1) Signature key
       (2) Encryption key
       (3) Authentication key
    Your selection?
    1

The `smart card` daemon `scdaemon` will then prompt you in a new window for your `pin`.

    scdaemon[6995]: DBG: asking for PIN '||Please enter the PIN'


For all of these keys we want to use `4096` so enter that.

    What keysize do you want for the Signature key? (4096)
    Key is protected.

Here you should be prompted (again outside of terminal) to enter your `diceware` passphrase.

    You need a passphrase to unlock the secret key for
    user: "Owen Kelly <owen@owenkelly.com.au>"
    4096-bit RSA key, ID 2F95015B, created 2016-01-19


All our `subkeys` should expire after 1 year.

You will need to regenerate your `subkeys` every year.

    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 1y
    Key expires at Wed 18 Jan 2017 04:08:18 PM UTC
    Is this correct? (y/N) y
    Really create? (y/N) y

Now you need to enter you're `admin pin`, again in popup.

    scdaemon[6995]: 3 Admin PIN attempts remaining before card is permanently locked
    scdaemon[6995]: DBG: asking for PIN '|A|Please enter the Admin PIN'

And now you should see a flashing light on your `yubikey` as it generates the key.

    scdaemon[6995]: please wait while key is being generated ...
    scdaemon[6995]: key generation completed (68 seconds)
    scdaemon[6995]: signatures created so far: 0
    scdaemon[6995]: signatures created so far: 1

GPG then finishes by relisting your keys metadata. You'll see a new entry prefixed with `sub`, that's your new `subkey`.

    pub  4096R/2F95015B  created: 2016-01-19  expires: never       usage: SC
                         trust: ultimate      validity: ultimate
    sub  4096R/225DF0AD  created: 2016-01-19  expires: 2017-01-18  usage: S
    [ultimate] (1). Owen Kelly <owen@owenkelly.com.au>

Now we do it again, but select `2` for `Encryption key`

    gpg> addcardkey
    Signature key ....: C3BD 2CB3 0A1F E35E 980D  11A7 AB02 51B5 225D F0AD
    Encryption key....: [not set]
    Authentication key: [not set]

    Please select the type of key to generate:
       (1) Signature key
       (2) Encryption key
       (3) Authentication key
    Your selection? 2

Again, you want a `4096` bit key.

    What keysize do you want for the Encryption key? (4096)
    Key is protected.

    You need a passphrase to unlock the secret key for
    user: "Owen Kelly <owen@owenkelly.com.au>"
    4096-bit RSA key, ID 2F95015B, created 2016-01-19

    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 1y
    Key expires at Wed 18 Jan 2017 04:09:53 PM UTC
    Is this correct? (y/N) y
    Really create? (y/N) y

    scdaemon[6995]: please wait while key is being generated ...
    scdaemon[6995]: key generation completed (44 seconds)

    pub  4096R/2F95015B  created: 2016-01-19  expires: never       usage: SC
                         trust: ultimate      validity: ultimate
    sub  4096R/225DF0AD  created: 2016-01-19  expires: 2017-01-18  usage: S
    sub  4096R/0F4A1F36  created: 2016-01-19  expires: 2017-01-18  usage: E
    [ultimate] (1). Owen Kelly <owen@owenkelly.com.au>

Finally generate the last `subkey`, choose `3` for `Authentication key`.

    gpg> addcardkey
    Signature key ....: C3BD 2CB3 0A1F E35E 980D  11A7 AB02 51B5 225D F0AD
    Encryption key....: 0C61 DDBE 49E3 C050 5170  8CE4 4A0F 42AE 0F4A 1F36
    Authentication key: [not set]

    Please select the type of key to generate:
       (1) Signature key
       (2) Encryption key
       (3) Authentication key
    Your selection? 3

    What keysize do you want for the Authentication key? (4096)
    Key is protected.

    You need a passphrase to unlock the secret key for
    user: "Owen Kelly <owen@owenkelly.com.au>"
    4096-bit RSA key, ID 2F95015B, created 2016-01-19

    Please specify how long the key should be valid.
             0 = key does not expire
          <n>  = key expires in n days
          <n>w = key expires in n weeks
          <n>m = key expires in n months
          <n>y = key expires in n years
    Key is valid for? (0) 1y
    Key expires at Wed 18 Jan 2017 04:10:54 PM UTC
    Is this correct? (y/N) y
    Really create? (y/N) y

    scdaemon[6995]: please wait while key is being generated ...
    scdaemon[6995]: key generation completed (44 seconds)

    pub  4096R/2F95015B  created: 2016-01-19  expires: never       usage: SC
                         trust: ultimate      validity: ultimate
    sub  4096R/225DF0AD  created: 2016-01-19  expires: 2017-01-18  usage: S
    sub  4096R/0F4A1F36  created: 2016-01-19  expires: 2017-01-18  usage: E
    sub  4096R/5DA2A8B2  created: 2016-01-19  expires: 2017-01-18  usage: A
    [ultimate] (1). Owen Kelly <owen@owenkelly.com.au>

Finally enter the `save` command to save the new subkeys in your `keyring`. This is important, if you skip this, you'll
need to regenerate your `subkeys`.

    gpg> save

<a name="publish-public-key"></a>

----

## 9. Publish your public key

Change directories `cd` to your `public USB`, in my case it's called `PUBLIC` and is located at `/media/PUBLIC`

    amnesia@amnesia:/~$ cd /media/PUBLIC

Then run `sudo gpg2 --armor --export owen@owenkelly.com.au >> 2F95015B-public.asc`, replacing your `user ID` and
`fingerprint` where appropriate.

    amnesia@amnesia:/media/PUBLIC$ sudo gpg2 --armor --export owen@owenkelly.com.au >> 2F95015B-public.asc

If you then `ls` the directory to *list all files*, you should see your new `public key`.

    amnesia@amnesia:/media/PUBLIC$ 2F95015B-public.asc


Now move your `public USB` to your `everyday computer` and import it. If you're on  OS X with GPG Tools installed, or
on Linux with GPG installed the following command will work:



<a name="next-steps"></a>

----

## 10. Next steps

- [How to use your PGP subkey for SSH authentication](/pgp/usage/ssh)
- [Generating new subkeys](/pgp/usage/new-subkeys)
- [Changing your passphrase](/pgp/usage/new-passphrase)
