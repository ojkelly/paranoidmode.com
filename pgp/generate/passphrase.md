# Generate your `diceware` passphrase

Passphrase? Don't you mean password?<br>
**No.**


A passphrase is a different to a password. Most people are terrible at making passwords. They're always too short,
and often are phrases commonly found in common use. Like, `to2ornot2b`. Literally the worst password after `12345`,
`qwerty` and well a few others like `dog1992`.

Passwords get their strength from [entropy](https://www.wikiwand.com/en/Entropy_(computing)), it's best thought of as randomness, or unpredictability. Encryption is easier and faster to crack when you can somewhat predict what the key might be.

I present one of the most famous XKCD's for your entertainment.

[![XKCD ](password_strength.png)](https://xkcd.com/936/)

This XKCD is a little misleading though, a good diceware passphrase needs more than 4 words. My current recommendation is to use `8 words` or more. If you can remember 10 words, use 10.

Here's what a 10 word passphrase looks like

    witt splay ash fried arena ryan apathy chief evoke kane

## **Let's make a diceware passphrase**

> There are websites that will generate diceware passphrases for you. There is only **one** way to make a diceware passphrase worth using. Dice.
> The problem with letting a computer generate it for you, is that is lacks the amount of entropy you can get with a dice and is significantly less secure as a result.

Download and print out [this Diceware list](dicewarewordlist.pdf) - it's 37 pages.<br>
You can also get this list from the [Diceware hompage](http://world.std.com/~reinhold/diceware.html).<br>
Or get a version [signed by the creator](http://world.std.com/~reinhold/diceware.wordlist.asc).<br>

You now have 37 printed pages which is where you will source a passphrase from. If you decide not to print out the list, **do not search through the page to find your words**. Scroll through the page and scan with your eyes.

Take your dice and roll 5 (or 1 dice 5 times) for every word you are going to generate.

For 8 words you'll roll 40 dice, for 10 words roll 50.

Find a nice spot where you can give your dice a good roll, something more than half a meter. I roll them on the floor.

Starting from the top write the numbers down on a new clean sheet of paper. The dice will not distribute evenly or in a neat line, so do your best to scan left to right starting from the top. Or any way really, just don't double up.

    64312552461253126346124315246512345164512465135156

Once you have all your numbers, you'll likely find it easy to group the numbers into clusters of 5.

    (64312)(55246)(12531)(26346)(12431)(52465)(12345)(16451)(24651)(35156)

After that translate the 5 digit numbers into their diceware word.

    (64312) = witt
    (55246) = splay
    (12531) = ash
    (26346) = fried
    (12431) = arena
    (52465) = ryan
    (12345) = apathy
    (16451) = chief
    (24651) = evoke
    (35156) = kane

Some people like to come up with a story to help them remember. This isn't my passphrase, and I'm not very good at the whole make a story thing, so enjoy this:

> **Witt** **splay**ed the **ash** while **fried** played in the **arena** with **ryan** who had **apathy** to the **chief**. Time to **evoke** **kane**.

The point with diceware passphrases are that they aren't common groupings of words. The result isn't something that follows normal speech. As that would be predictable and less secure. Your passphrase story is just a method to make the keywords a bit less nonsensical so you can remember them.

You should now have a sheet of paper with 40 or 50 numbers on it, and your diceware passphrase below it.
Keep this paper  safe. Fold it up and put it in your wallet. You will need to refer to it for a couple of days until you memorize the password. After that you might want to store it with your `private key` backups.
If you forget this passphrase you will loose access to your private key. If you loose this passphrase, you can change it.
