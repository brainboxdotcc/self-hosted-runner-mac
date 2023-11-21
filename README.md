# self-hosted-runner-mac
Self-hosted mac runners are no longer suitable to be ran through docker. There are many reasons for this, the important ones being:
- The project that we were using for mac containers (MacOSContainers) does not currently have xcode support.
- MacOSContainers requires security features to be disabled and has an exploit allowing people to escape the container, which is a great risk for us.
- MacOSContainers does not allow you to currently create other users, meaning we can not lockdown security.

Instead, we are now using a Virtual Machine. This Virtual Machine requires 2 cores, 2GB of ram, 48GB of storage, and **requires an Apple Silicon CPU**.

This Virtual Machine requires you to do the following steps in-order to set it up:

- Download the Repo and load the xcode project on your host machine (this is the machine that will be running the Mac).
- Inside xcode, click the project in the project files (this looks like the TestFlight app logo).
- From here, head to "Signing & Capabilities" and sign all the **Objective-C** targets under your team. It's important that you sign the Objective-C targets, as these are the targets you will be using.
- At the top centre of xcode, you can now switch the target. Change it to "InstallationTool-Objective-C". You can now hit the Play button to the left. This will download a VM restore image from apple to your home folder, under the name `VM.bundle`. **This step will use 4GB of ram, after the installation has completed, to quickly boot the VM and close it. Anything less than 4GB of ram fails the installation. The step below will automatically lower the ram to 2GB**
- Now, change the target to "macOSVirtualMachineSampleApp-Objective-C" and hit the Play button to the left.
- You should see the VM boot up and you will be able to begin the setup process.
- Skip the migration assistant and the apple login and then agree to the terms.
- Setup the VM with the name "runner" (the name technically isn't required, but is useful to keep it as runner).
- Setup a password (this can be completely private and nobody needs to know the password but you).
- Do not allow location services.
- You can either pick a random location for the time or just continue with America, we will be changing this soon.
- Opt out of analytics and then skip the screen time setting. Now, you can pick if you want light or dark.
- Open terminal and run `sudo ln -sf /usr/share/zoneinfo/UTC /etc/localtime`, this will force your VM into using the UTC timezone. You **need** to do this step. If you skip this step, `ts_not_null` will most likely fail. Note, you can't copy paste into your VM, so open this repo's readme on the actual VM (you can do this in your web if you would like).
- Open settings, find "Lock Screen" and turn off "Require password..." and "Start Screen Saver..."
- Download brew from https://brew.sh (this will install xcode).
- Follow the instructions from brew (after the brew installation) to apply brew to the PATH.
- Download initialise.sh from this Repo, move it out of your downloads folder (to your home folder), run `chmod +x ./initialise.sh`, and run the file like so `./initialise.sh`.
- Once that's finished, do `cd actions-runner` and then run the config like so `./config.sh --url https://github.com/brainboxdotcc/DPP --token <token>`. To get a token for DPP, contact brain on discord.
- After that, you can do `./run.sh` and the runner should be alive!

# Todo:
- Look to possibly make the image come with brew added and the runner ready to be setup?
- Add a step for doing `run.sh` on boot.
- Look to make the VM not contain an interface (this will save a lot of performance). 
