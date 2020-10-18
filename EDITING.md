# How to edit the HUD code

## Prerequisites for local development

If you are on Windows, you will need WSL2 installed:
1. Install WSL2 with Ubuntu (v20 is fine): https://docs.microsoft.com/en-us/windows/wsl/install-win10
1. Install VSCode with the Remote Development Pack: https://code.visualstudio.com/docs/remote/wsl
1. Close and reopen your WSL window if you have one open

Everything from here after assumes you are running in the Linux environment
1. Update your package list: `sudo apt update`
1. Install lua: `sudo apt install lua5.3`
1. Install NPM (Node.js): `sudo apt install npm`
1. Create an SSH key for GitHub: `ssh-keygen -t rsa -b 4096 -C "<your email>" -f ~/.ssh/<your_name>_rsa`
1. Update your SSH config file to use the key:
```
cat >> ~/.ssh/config <<EOF
Host *
    Hostname github.com
    User git
    IdentityFile ~/.ssh/<your_name>_rsa
EOF
```
1. Add the SSH Key to GitHub (see https://devconnected.com/how-to-setup-ssh-keys-on-github/#Add_SSH_key_to_your_GitHub_Account if you don't know how to do this)
1. Clone the repo: `git clone git@github.com:Dimencia/DU-Orbital-Hud.git`
1. Set your git name and email for commits:
```
git config --add user.name=<your name>
git config --add user.email=<your email>
```
1. Update your local packages in the root of the git repo: `npm install`
1. Run VSCode: `code .`
1. Install the `Lua` and `Lua Helper` extensions in VSCode

Then, when you are ready to create the minified version of the configuration, just do: `CTRL`-`SHIFT`-`B` and select `wrap`.
`ButtonHUD.conf` will be updated with the minified version, which you can then deploy as usual.

## Creating the wrapped (deployable) `ButtonHUD.conf`

The HUD autoconf is generated from `ButtonHUD.lua`, which is the master implementation.  Note that this is organized
according to how `wrap.lua` expects, which is documented [here](https://board.dualthegame.com/index.php?/topic/20161-lua-tool-script-packagerconfigurator-wraplua/).

The `ButtonHUD.conf` file is generated by `wrap.sh`, which does the the following:
* Extracts exports and version information
* Runs the lua file through the `luamin` minifier, removing unnecessary whitespace and collapsing symbol names
* Runs that through `wrap.lua` which creates the DU AutoConf from the script
* Updates the conf file with previously extracted exports and fixes up the L_TEXT localization macros

The above process may be run in one of the following two ways:
* `./wrap.sh true`
* In VSCode: `CTRL`-`SHIFT`-`B` and select `wrap`

The resulting `ButtonHUD.conf` can be deployed to DU using the regular installation instructions.
> NOTE: You should not check in manual edits to `ButtonHUD.conf` as they will be discarded the next time the wrapping scripts are run.
