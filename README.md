# Paranoid Mode

An opinionated guide to setting up and using PGP keys in a post-Snowden world.

I wrote for a `threat model` of either a nation state or close enough to one. As such is may need more hardware than you expect. Don't let that deter you.

The hardware required can be purchased for less than $150USD. If you have an old computer lying around you can probably use that.

You should follow this guide if you are a **programmer**, **journalist**, **network administrator** or **sysadmin**, or someone who just uses `ssh` regularly.

----

### **Assumed Knowledge**

Writing a guide like this, it's very hard to cater to all knowledge levels. This guide is written with the following assumptions:

  - You are using Linux, Unix or OS X (accepting pull requests for Windows instructions)
  - You can use terminal
     - You don't need to have mastered it, but you need to know what it is, and how to open it up


### **Some things you might not know**

  - `crtl-c` will cancel a running command in terminal
    - useful when something gets stuck, you hit the wrong command or you need to stop something
  - `up` and `down` let you cycle recent commands
  - In `Tails` and other Linux terminals to copy text from the terminal you use `Shift-Ctrl-C` and `Shit-Ctrl-V`
  - In `Tails` you need to be root to access an OpenPGP smartcard (that's the Yubikey)

You should also know that there are known malware programs who's only job is to scan everything it can looking for `private keys`. These keys are super obvious as they always start with:

      -----BEGIN PGP PRIVATE KEY BLOCK-----

So yes, be paranoid about your private key touching any networked computer, it will take barely a second for the key to be exfiltrated, and rendered insecure. And you will likely never know if this has actually happend.

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

### **What you will be able to do**

After completing this guide you will be able to:

 - receive and `decrypt` files and emails encrpyted with your `public key`,
 - `sign` files, text and other peoples public keys
 - `authenticate` yourself using PGP

