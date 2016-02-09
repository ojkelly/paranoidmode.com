# Backup your private key

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

