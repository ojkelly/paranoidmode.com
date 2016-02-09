# How to use your OpenPGP SmartCard for SSH Authentication

How to use your PGP key and `OpenPGP Smart Card` to authenticate with SSH.


## Server

To use your `subkey`, you need to export the `public key` of your `authentication subkey`, in a format that `ssh` can use.

If you don't know the fingerprint of your `authentication subkey` you can open up `--edit-key` to find out. In this case, we are editing the root key, so use that fingerprint.

    owen@everydayLaptop:~$  gpg2 --edit-key 2F95015B
    gpg (GnuPG) 2.0.29; Copyright (C) 2015 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Secret key is available.

    pub  4096R/2F95015B  created: 2016-01-19  expires: never       usage: SC
                         trust: ultimate      validity: ultimate
    sub  4096R/225DF0AD  created: 2016-01-19  expires: 2017-01-18  usage: S
    sub  4096R/0F4A1F36  created: 2016-01-19  expires: 2017-01-18  usage: E
    sub  4096R/5DA2A8B2  created: 2016-01-19  expires: 2017-01-18  usage: A
    [ultimate] (1). Owen Kelly <owen@owenkelly.com.au>

    gpg>

In the output of that command we get a list of all the subkeys, and what they can be used for.
Look for the `subkey` with `usage: A`. In this case it's fingerprint is `5DA2A8B2`.


Type `quit` to exit `--edit-key`

There's a great program called `gpgkey2ssh` which does exactly what it says. It takes a
`gpg` key, and formats it for use with `ssh`. I've trimmed the output below, but as it's
your public key it isn't secret. You should grab the output of this function and add
it to `~/.ssh/authorized_keys` on every server you need to ssh into.

    owen@everydayLaptop:~$ gpgkey2ssh 5DA2A8B2
    ssh-rsa AAAAB...== owen@owenkelly.com.au

## OS X

First add support for `ssh` in `gpg-agent`:

    owen@everydayLaptop:~$ nano ~/.gnupg/gpg-agent.conf

    enable-ssh-support
    write-env-file
    use-standard-socket
    default-cache-ttl 600
    max-cache-ttl 7200


Add the following to your `.bashrc` or `.zshrc` profile:

    if [ -f "${HOME}/.gpg-agent-info" ]; then
      . "${HOME}/.gpg-agent-info"
      export GPG_AGENT_INFO
      export SSH_AUTH_SOCK
    fi
    export GPG_TTY=$(tty)


This will start `gpg-agent` for you.

Now we need to get `ssh-agent` and `gpg-agent` talking to each other.

First run `ssh-add -L` to list all identities in `ssh-agent`.

    owen@everydayLaptop:~$ ssh-add -L
    ssh-rsa AAAABadadasdasdasda...truncated /Users/owen/.ssh/id_rsa

If you get back a response pointing to a file in the `~/.ssh` directory, it means you currently have a `ssh keypair` setup. We need to unlink this from `ssh-agent` in order to use your `gpg` keypair`.

Type `ssh-add -D` to remove all identities in `ssh-agent`.

    owen@everydayLaptop:~$ ssh-add -D
    All identities removed.


If you run `ssh-add -L` you should be told there are no identities now.

    owen@everydayLaptop:~$ ssh-add -L
    The agent has no identities.

Now we're ready to being using `gpg-agent` with `ssh-agent`.


    owen@everydayLaptop:~$ ssh-add -L
    error fetching identities for protocol 1: agent refused operation
    ssh-rsa AAAAB3NzaC... == cardno:000000000000


Start a new terminal window and `ssh` into the server you just added your public key to.

If you don't see your key with `carno:000000000...` on the end you need to restart `gpg-agent`. You only need to do
this now though, as the next time the system boots, the new `gpg-agent.conf` will be loaded.

To restart `gpg-agent`, first stop it, then start a new one.

    sudo pkill -9 gpg-agent

    gpg-agent --daemon
