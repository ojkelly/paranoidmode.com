# Generate revocation certificate

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


