# Generate your root PGP key


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

