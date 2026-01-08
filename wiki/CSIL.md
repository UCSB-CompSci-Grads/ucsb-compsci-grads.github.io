# Remote access to the UCSB computing labs (and some SSH tricks)

**Note: Unless otherwise denoted, all commands are the same for Windows, Linux, and MacOS.**

## First time setup

- Create a College of Engineering (CoE) account at this link: [https://eci.ucsb.edu/account-creation-links](https://eci.ucsb.edu/account-creation-links)
- If you have any issues with your account or if you forget your password, go back to the link above.

## Logging in thorough SSH

(This section assumes you have some experience using Powershell/the terminal/the command line.)

1. Log in with `ssh <your COE username>@csil.cs.ucsb.edu`.
2. You will be prompted for your password - this is your CoE password created above.

## Adding a public/private key pair for CSIL

Adding a key pair allows you to avoid entering a password every time you log in to CSIL.

1. Create a key pair with `ssh-keygen`.
2. Follow the prompts to save the keypair and add a password to the file (not required if you're on a personal computer, recommended on a shared computer).
3. Navigate to the location of the keypair and copy ONLY the public key to CSIL using the command `scp <path/to/file> <COE username>@csil.cs.ucsb.edu:~/.ssh/authorized_keys`
4. Enter your password when prompted.

If all has gone well, you should now be able to log in to CSIL without a password.

*note: You can do this on any remote server, just replace `<COE username>` with your username on that server and "`csil.cs.ucsb.edu`" with the public IP for that server.*

## Adding an alias for CSIL

If you want to type even less, try setting up an alias for CSIL:

1. Open/create a file named `config` in the SSH folder.
  - **Linux or Mac:** Located at `~.ssh/config`
  - **Windows:** Located at `%userprofile%\.ssh\config`
2. Add to the file:
```
Host <nickname>
Hostname csil.cs.ucsb.edu
User <your COE username>
IdentityFile <path to your private key>
```
*note: You can do this multiple times to add additional servers!*

3. Save the file.

4. Test it by running `ssh <nickname>` on the command line. You should now be connected to CSIL.

## Connecting to CSIL using VS Code

Microsoft has a tutorial for remote development [here](https://code.visualstudio.com/docs/remote/ssh). I will also summarize here the key points:

1. Install the **Remote-SSH** extension in VS Code.
2. Open the command palette (**F1** or **CTRL+Shift+P**) and select **Remote-SSH: Connect To Host...**
3. Use the same ssh command as above (`ssh <nickname>` or `ssh <COE username>@csil.cs.ucsb.edu`)

## Help! My CSIL userspace is running out of space when I use VS Code!

**Warning: Double check your paths when using these commands!**

This is a known issue with VS Code. To fix this, navigate to your home folder on CSIL (`cd ~`) and run `ls -a`. *If* you see `.vscode-server`, run `rm -rf ~/.vscode-server`. **Ensure that the path is correct before running! `rm -rf` forces (`-f`) removal of all files in that path recursively (`-r`)! All files within this folder will be permanently deleted!**

Next time you connect to CSIL with VS Code, it will reinitialize the `.vscode-server` folder, but it should be significantly smaller this time.
