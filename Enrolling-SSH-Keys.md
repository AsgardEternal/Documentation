# Enrolling SSH Keys

An ssh key is used to login to our server otherwise known as the "box". SSH is used to administer the server through a command line/terminal session.

> [!IMPORTANT]
> Nobody, **_NOBODY_**, should **ever** ask for your SSH _private_ key. We use the public key and expose them for login to the server, but during the key generation process you also create a private key which is used to authenticate against the public key. As in the name, "private", don't ever share that key. There is no legitimate reason anyone will ever ask for that key nor is there a legitimate reason you should ever share that private key. Assume impersonation/an attack if anyone asks for the private key or attempts to get access to it and report it immediately.

> [!NOTE]
> All the below is aimed at Windows users, the steps are largely the same on MacOS/Linux, the only real difference is using something other than Powershell as your shell and the home directory of the user.

## Create the SSH key

+ Open Powershell
+ Type `ssh-keygen -t ed25519`
+ Hit _enter_ to accept the path where to save the key
  - If you are prompted to Overwrite, **STOP!** Validate that the key in the given path is not important, if it is, use a different path to save your new key.
+ It's recommended to use a passphrase to protect the key, but given our security stance it's not a big deal if you don't decide to use a passphrase, you can use an empty passphrase

## Get your public key and add it to Ansible ([Ubuntu-Server-Setup](https://github.com/AsgardEternal/Ubuntu-Ansible-Setup))

> [!WARNING]
> This is **_NOT_** your private key. If you expose the private key, you'll have to do all of this over again.

+ At the path where SSH generated your key, there should be a similar file name as the key you generated but suffixed with `.pub`
+ Copy the contents of that file
+ Head on over to the [Ubuntu-Server-Setup](https://github.com/AsgardEternal/Ubuntu-Ansible-Setup) repository
+ Open the `inventory.yml` file in the top level of that repository
+ Go to the bottom of the file and replace/add your public key to the `asgard` superuser
+ Save the file and commit the changes

## Configure SSH to use the new key
> [!NOTE]
> The below will overwrite your SSH config, so if there's anything you _know_ to be important, you may want to back it up.

+ In Powershell you can paste the following to setup your config:
  ```powershell
  New-Item -Path "$env:USERPROFILE\.ssh\config" -ItemType "file" -Force
  Set-Content -Path "$env:USERPROFILE\.ssh\config"  -Value @'
  Host asgard
    User asgard
    HostName asgard-eternal.com
  Host asgard-eternal.com
    User asgard
    HostName asgard-eternal.com
  '@
  ```
## Validate you can login

+ Attempt to login to the server by typing `ssh asgard@asgard-eternal.com` into Powershell and hit _enter_
+ If you successfully logged in, then congrats, you have now deployed a new SSH key
  - If your login failed, contact Price and he'll take a look


