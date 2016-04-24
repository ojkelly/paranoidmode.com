# Automatically lock your computer when you remove your OpenPGP Smart Card


## OS X

You can use [Hammerspoon](http://www.hammerspoon.org/) and the little script below to automatically lock your computer when you remove your Yubikey. It also sends you an email.


Add the code below to `~/.hammerspoon/init.lua`.

    local usbWatcher = nil

    -- lock when yubikey is removed
    function usbDeviceCallback(data)
    	if string.match(data["productName"], "Yubikey") then
            if (data["eventType"] == "added") then
            	hs.alert.show("You just connected your " .. data["productName"])
            elseif (data["eventType"] == "removed") then
            	hs.messages.iMessage("example@example.com", "Your yubikey was just removed.")
            	os.execute("/System/Library/CoreServices/Menu\\ Extras/User.menu/Contents/Resources/CGSession -suspend")
           end
       end
    end

    usbWatcher = hs.usb.watcher.new(usbDeviceCallback)
    usbWatcher:start()


> If you have a method for doing this on an other OS please add a comment, or submit a pull request.