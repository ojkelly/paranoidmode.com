# Subkeys

Your sub-keys, like your root keypair, are a normal key pair (a paired public key and private key). Unlike your root key, they cannot `certify` other keys. That capability is reserved exclusively for the root keypair.

Sub-keys are mostly ephemeral. You will replace them every year.

You should never have a copy of the private key of a sub-key, rather it should be generated and stored solely on **one** smartcard. If the smartcard is lost, stolen or fails, you can revoke the sub-keys (using your root keypair), replace the smartcard, and generate new sub-keys &mdash; **in that order**.

## How they work

Using your root keypair you ask a smartcard to generate new keys and send the public key back. The public key is then signed with your root keypair. You do this for each subkey typically 3, one each for `encryption`, `signing`, and `authentication`. Then you publish your updated root keypair with the signatures for your new subkeys.

Now you have 3 key pairs cryptographically guaranteed to be under your control, (as we signed them with your root keypair).

## Why we use them

They're much easier to use than using your root keypair all the time. If you didn't have subkeys every time you wanted to use your root keypair you would need:
 - to boot your secure airgapped computer
 - transfer the information to be encrypted
 - unlock your private key
 - decrypt or sign the information
 - destroy the private key on the computer
 - transfer the information back to your normal computer.

Why so many steps? Why can't you just keep the whole key pair on your computer?

Keeping the keypair on your computer is actually common place for `ssh`, but it shouldn't be. It's trivial to exfiltrate keys, aside from all looking the same, they're almost always in the same place `~/.ssh`.

With sub-keys, you have a USB device that physically stores the private key component of your sub key. You need to maintain physical control of the smartcard, but that is exponentially easier than maintain control of a keypair sitting on a computer.

Futher, by storing your sub-keys on a smartcard the private keys are never published. A good smartcard makes it technically impossible to extract the private keys. That's not to say there may not be a way, the goal is to make it prohibitively dangerous and expensive.

Smartcards also have very low pin failure limits. If you guess your pin wrong 3 times, and then guess your admin pin (PUK) wrong 3 times, the card will erase itself.

If that happens you will forever lose access to those sub-keys, and you will need your root key to generate new ones.