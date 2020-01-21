## Boot Tails on your air-gapped laptop

> **STOP AND READ THIS BEFORE CONTINUING**
>
> Before booting into Tails ensure that there are no wireless transmitters in the laptop (wifi, bluetooth, cellular).
>
> Make sure there is **no hard drive** or **storage media** excluding your USBs connected or installed in the hard drive.
>
> Remember we're working with your private key on this laptop, as such we must take every precaution to
ensure it cannot be exfiltrated.

**Did you read the big message above? You did? So there's no wireless radios or data storage media other than your USBs?**

If you cheap out here you might be putting yourself at risk.
You should be able to get a super crappy laptop for $150 or less off ebay.
You barely need anything to run Tails. So get a dedicated laptop for airgapped work.

Okay let's move on.

Plug your Tails USB into your laptop and boot from the USB. You might need to toggle the boot order in the BIOS to set the USB drive as the first boot media.

When you boot Tails you're greeted with a pre-launch wizard that gives you some options. The only thing you need to do is set an `admin password`. There are other options you can set but as this device never touches a
network you can just set the admin password and launch Tails.

You need an admin password set so that you can use `sudo` to interact with the `OpenPGP Smart Card`. The drivers don't allow you to interact if you're not `root` (which is what `sudo` does).

A quick and easy way to mount a USB drive with Tails is to open it with the <abbr title="Graphical User Interface">GUI</abbr>.

> TODO Add instructions regarding printing, and checking that you can print.

### Prepare your USBs

Grab your two left over USBs. Designate one as your `private USB` and the other `public USB`. If they are physically different
this should be easy, otherwise mark one to indicate the difference.

It's handy to format these two drive to be labeled `PRIVATE` and `PUBLIC`. That way you won't get mixed up moving files around.

**You need to ensure your `private USB` never connects to anything other than your airgapped laptop.**

Now to mount your `private USB` and `public USB`.
