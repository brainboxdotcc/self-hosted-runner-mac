# self-hosted-runner-mac
Self-hosted mac runners are no longer suitable to be ran through docker. There are many reasons for this, the important ones being:
- The project that we were using for mac containers (MacOSContainers) does not currently have xcode support.
- MacOSContainers requires security features to be disabled and has an exploit allowing people to escape the container, which is a great risk for us.
- MacOSContainers does not allow you to currently create other users, meaning we can not lockdown security.

Instead, we are now using a Virtual Machine. This Virtual Machine requires 2 cores, 2GB of ram, 128GB of storage (subject to change, expected to lower to 32GB), and **requires an Apple Silicon CPU**.

This Virtual Machine requires you to do the following steps in-order to set it up:

- Download the VM from https://developer.apple.com/documentation/virtualization/running_macos_in_a_virtual_machine_on_apple_silicon and follow their guide to get it started.
- Setup the VM with the name "runner" (the name technically isn't required, but is useful to keep it as runner).
- Setup a password (this can be completely private and nobody needs to know the password but you).
- Do not allow location services, skip the apple login, etc.
- Open terminal and run `sudo ln -sf /usr/share/zoneinfo/UTC /etc/localtime`, this will force your VM into using the UTC timezone. You **need** to do this step. If you skip this step, `ts_not_null` will most likely fail.
- Download brew from https://brew.sh (this will install xcode).
- Follow the instructions from brew (after the brew installation) to apply brew to the PATH.
- Download initialise.sh from this Repo and run the file like so `./initialise.sh`.
- Once that's finished, do `cd actions-runner` and then run the config like so `./config.sh --url https://github.com/brainboxdotcc/DPP --token <token>`. To get a token for DPP, contact brain on discord.
- After that, you can do `./run.sh` and the runner should be alive!

# Todo:
- Add the VM xcode project here (if permitted by Apple)
- Look to possibly make the image come with brew added and the runner ready to be setup?
- Add a step for doing `run.sh` on boot.
- Look to make the VM not contain an interface (this will save a lot of performance). 
