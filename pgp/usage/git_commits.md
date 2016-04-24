# Signing Git commits

To sign a git commit add `-S` to the end of your `git commit` command.

    git add newfile.md
    git commit -m 'Added new file' -S
    
You will then be asked for your pin for your smartcard. Enter it then you should see git output a message about the commit.