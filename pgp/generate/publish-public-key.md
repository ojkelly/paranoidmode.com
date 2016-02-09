# Publish your public key

Change directories `cd` to your `public USB`, in my case it's called `PUBLIC` and is located at `/media/PUBLIC`

    amnesia@amnesia:/~$ cd /media/PUBLIC

Then run `sudo gpg2 --armor --export owen@owenkelly.com.au >> 2F95015B-public.asc`, replacing your `user ID` and
`fingerprint` where appropriate.

    amnesia@amnesia:/media/PUBLIC$ sudo gpg2 --armor --export owen@owenkelly.com.au >> 2F95015B-public.asc

If you then `ls` the directory to *list all files*, you should see your new `public key`.

    amnesia@amnesia:/media/PUBLIC$ 2F95015B-public.asc


Now move your `public USB` to your `everyday computer` and import it. If you're on  OS X with GPG Tools installed, or
on Linux with GPG installed the following command will work:

