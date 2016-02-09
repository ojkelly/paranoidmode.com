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
   - Ideally you want 8, but 1 will do
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
      Yubikeys don't support `OpenPGP Smart Card`. Make it easy for yourself, get a new Yubikey 4 or newer.
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

