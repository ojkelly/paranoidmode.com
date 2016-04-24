# How to encrypt a file for someone else

Let's say you're going to send me a file.

First you need my public key, so let's go and get that.

    gpg2 --recv-keys 2F95015B
    
Where did `2F95015B` come from? It's the shortkey of my public key. You will need the find the appopriate one for the person you wish to encrypt for. This is not a small topic, so first read [how to find someone's public key](usuage/how_to_find_someones_public_key.md).