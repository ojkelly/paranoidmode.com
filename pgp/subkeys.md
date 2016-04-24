# Subkeys

Your sub-keys, like your root key, are a normal key pair (a paired public key and private key). Unlike your root key, they cannot `certify` other keys. That capability is reserved exclusively for the root key.

Sub-keys are mostly ephemeral. You will replace them every year.

You should never have a copy of the private key of a sub-key, rather it should be generated and stored solely on **one** smartcard. If the smartcard is lost, stolen or fails, you can revoke the sub-keys (using your root key), replace the smartcard, and generate new sub-keys &mdash; in that order.

## How they work



## Why we use them

