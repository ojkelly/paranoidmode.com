# Generate your subkeys

It's now time to plug in that `yubikey` or other `OpenPGP Smart Card`.

Make sure you know what the `pin` and `admin pin` are.

Firstly change back to your home directory by entering `cd ~`.

Then run `sudo gpg2 --card-status` to check if your `smart card` is working. You should see an output similar to below.

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

Now we know your `smart card` is working we can generate the `subkeys`.

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

Type `addcardkey` to get started creating the first `smart card` key pair.
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

Now you need to enter your `admin pin`, again in a popup.

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
    