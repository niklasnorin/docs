---
title: Raspberry Pi (all models)
---

# Getting Started With the Raspberry Pi

This guide will walk you through setting up any of the Raspberry Pi devices on resin.io.
At the time of writing the A/A+, B/B+, RPI2 are supported.

## What You'll Need

* A Raspberry Pi model [B][rpi-b], [B+][rpi-b-plus], [A+][rpi-A-plus], [Raspberry Pi 2][RPI2-link] or [Raspberry Pi 3][RPI3-link].
  See our [supported devices][supported] for a full list of the boards we currently support.
* A 4GB or larger SD card.
  The [Raspberry Pi][rpi] uses a Micro SD card. The [speed class][speed_class] of the card also matters - this determines its maximum transfer rate. We strongly recommend you get ahold of a class 10 card or above.
* An ethernet cable or [WiFi adapter][wifi] to connect your device to the
  internet.
* A micro USB cable.
* Some awesome ideas to hack on! If you need inspiration, check out our
  [projects][projects] page.

<!-- ========================== Generic ALL devices =================================   -->

__NOTE:__ If you're not experienced with [git][git], check out the excellent
[Try Git][try-git] course at [Code School][code-school].

If you already have a resin.io account and just want to get started with your new device, then skip ahead to [Creating Your First Application](/installing/gettingStarted#creating-your-first-application).

## Signing Up

Enter your details on the [sign up page][signup]. There are a couple of
restrictions:

* The username can only contain letters and numbers.
* The password has to be at least 8 characters long.

<img src="/img/common/sign_up_flow/sign_up_cropped.png" width="80%">

## SSH Key

SSH keys use the power of [public-key cryptography][pub_key_crypto] to secure
your connection when sending your code to us. In order to secure your [git][git]
connection, we need your __public__ [SSH Key][ssh_key] (you must never share
your *private* key with anyone.)

Simply paste your public key into the box provided on the UI and click `save`. Alternatively you can import your key from [Github][github]. If you don't have an ssh key or have never used one, we recommend you take a look at [Github][github]'s [excellent documentation][github_ssh] on the subject and how to generate a key pair for your platform.

<img src="/img/common/sign_up_flow/enter_ssh_key_cropped.png" width="80%">

Once generated, SSH keys are easy to use. In fact you generally don't have to
think about it at all. Once you're set up just `git push` your code to us and
it's taken care of automatically and securely.

If you don't have your ssh key setup yet, but want to explore resin.io, just click `skip`. Note that you will not be able to push code to your devices until you have an ssh key saved. This can be done at anytime from the `Preferences` page on the dashboard.

### Import SSH key From GitHub

For convenience we provide the ability to import your public SSH key from
[GitHub][github] - just click on the Octocat icon in the bottom right-hand
corner ([we use][github_ssh_blogpost] GitHub's [public APIs][github_apis] to
retrieve this data.)

You will then have to enter your github username:

<img src="/img/common/sign_up_flow/enter_github_username_cropped.png" width="60%">


## Creating Your First Application

The two key components you will interact with in resin.io are *applications* and
*devices* - applications represent the code you want to run on your devices, and
devices are the actual hardware itself.

You can have as many devices connected to an application as you like - when you
push code, resin.io deploys to every device that is part of that application.

To create your first application simply type in a name, select as your device type and click the create button. You should now be taken to the dashboard of your newly created application:

<img src="/img/common/main_dashboard/select_fleet_type.png" width="80%">

<!-- ========================== end section =================================   -->

<!-- ========================== Device Specific =================================   -->

__Warning:__ Each Raspberry Pi model has its own device type, since they use slightly different CPU architectures.

## Adding Your First Device

This is the application dashboard where all of the devices connected to your
application will be shown, along with their statuses and logs.

<img src="/img/common/app/app_dashboard_empty.png" width="80%">

Click the `Download Device OS` button to get the resin.io operating system image for your application. A dialog will appear prompting you to specify how your device connects to the internet - either via an ethernet cable or wifi, in which case you can specify your Wifi network's SSID and passphrase. Click the `Download Device OS` button to get the resin.io operating system image for your application.

<img src="/img/common/network/network_selection_wifi_cropped.png" width="60%">

<!-- ========================== end section =================================   -->

<!-- ========================== Generic for SD devices =============================   -->

While the file downloads ensure your SD card is formatted in [FAT32][fat32]
([WikiHow][wikihow] has [instructions][wikihow_format] on how to do this).

Once the download is finished you should have a `.img` file with a name like `resin-myFleet-0.1.0-0.0.16-b2854a2c7639.img` where "myFleet" is the name you gave your application on the dashboard.

Now we have to burn the downloaded `.img` file onto the SD card. There are a couple of ways to do this depending on your host computer's operating system. We have listed a few below.

## Burning the OS image onto the SD card

* [Mac and Linux Command Line](/installing/gettingStarted#on-mac-and-linux)
* [Windows](/installing/gettingStarted#windows)

### On Mac and Linux

#### From the command line

First we need to figure out what our SD card is called. To do this open a terminal and execute the following command to see the list of connected storage devices:
`df -h`
Next, insert your microSD card and execute the following command again:
`df -h`
Compare the two outputs and find the newly added device. In my case, the microSD card was '/dev/disk2s1'.
Depending on your OS, this device can take different names, like '/dev/mmcblk0p1' or '/dev/sdb1'.

Once you've got the name of the SD card, you'll want to unmount that disk using the following command, replacing the specifics with your card details:
`sudo diskutil unmount /dev/disk2s1`
Now you'll want to execute the command that actually copies the image onto the SD card.

__Warning:__ You have to be really careful here, and make 100% sure you are entering the correct SD card details. You could end up copying over the wrong drive (such as your master hard disk) and then you're gonna have a bad time. Double check everything!

Also, choose the right file location for your `.img` file in the input file field (if=...).
`sudo dd bs=1m if=~/Downloads/resin-myFleet-0.1.0-0.0.16-b2854a2c7639.img of=/dev/rdisk2`

__NOTE:__ that we subtly changed the device name from "/dev/disk2s1" to "/dev/rdisk2". You'll want to do the same when you execute the above command, with the corresponding device name. If your device name is "/dev/mmcblk0p1", use "/dev/mmcblk0". If it's "/dev/sdb1", use "/dev/sdb". The idea is to use the device name for the whole SD card and not just a partition. If you're not sure, use `ls /dev` before and after inserting the card and note the difference.

__NOTE:__ Linux users will need to run `sudo dd bs=1M if=~/Downloads/resin-myFleet-0.1.0-0.0.16-b2854a2c7639.img of=/dev/sdb` (uppercase M)

This process can take anywhere from 5-30 minutes depending on the speed of your computer and microSD card. Once this is done, skip down to [setting up your device](/installing/gettingStarted#setting-up-your-device)

#### From a GUI

Alternatively you can use the GUI program [PiFiller][pifiller-download] to burn the SD card.

Once downloaded, launch Pi Filler, and follow the on-screen prompts. The first thing it will ask is for you to locate your `.img` file.

Locate your resinOS `.img` file in your Downloads folder. It should be named something like `resin-myFleet-0.1.0-0.0.16-b2854a2c7639`. Now click "choose".

You can now insert your microSD card into your host machine and click continue. PiFiller will look for your SD card and tell you when it finds it.

__Warning:__ Make 100% sure that the SD card it finds is in fact the correct card.

Click continue and piFiller will write to the SD card. This can take 5-25 minutes depending on your machine. Once this is done, skip down to [setting up your device](/installing/gettingStarted#setting-up-your-device).

### Windows

To burn OS images to SD cards on Windows, you will need to install [Win32 disk imager][win32-disk-imager]. Once you download it, you can launch win32 disk imager by clicking on the "Win32DiskImager" file in the folder that you extracted it to.

Now in Win32DiskImager, click on the folder icon to select which `.img` file you wish to burn. A file browser window will open and you will need to select your resinOS image from the Downloads folder. It should be the extracted version and named something like this `resin-myFleet-0.1.0-0.0.16-b2854a2c7639.img`.

Next insert your SD card into your host computer and in the Win32DiskImager GUI, select your SD card when it appears.

__Warning:__ Be very careful to make sure that you have selected the right SD card. Double check this!! Otherwise you could end up writing over your host machine's hard disk.

Once you have made your selections and are 100% sure you are writing to your SD card and nothing else, you can click write and wait for the SD card to be burned.

Once it is completed, you can carry on setting up your device as shown below.

<!-- ========================== end section =================================   -->

<!-- ========================== Device Specific =================================   -->

## Setting Up Your Device

Put the SD card into your device and connect either the ethernet cable or WiFi adapter. If you're connecting the cable to your computer, you'll need to enable connection sharing. Now power up the device by inserting the USB cable.

![insert SD](/img/gifs/insert-sd.gif)

It will take a few minutes for the Raspberry Pi to appear on your resin.io dashboard, so take this time to grab some tea while you wait.

While you wait resin.io is expanding the partitions on your SD card, installing a custom linux environment and establishing a secure connection with our servers.

If you have a class 10 SD card and a fast internet connection your device should appear on the dashboard in around 7 minutes. Note that Class 4 SD cards can take up to 3 times longer so it's well worth investing in the fastest card you can find.

__Note:__ If you have an HDMI screen attached, you should see `"Booted - Check your resin.io dashboard."` on the screen when the device boots. If instead you see rainbow colors or a black screen with a raspberry on it, it could mean that the SD card was not burned correctly or is corrupted. Try [burning the SD card](/installing/gettingStarted#burning-the-os-image-onto-the-sd-card) again. If the issue persists, click the little yellow ` ? ` on in the bottom right of the resin.io dashboard and speak to our support engineers.

<!-- ========================== end section =================================   -->

<!-- ==================== Code Example (text2speech)==========================   -->

## Running Code On Your Device

A good first project is our [text to speech app][example_app]. This simple node project converts text to speech and plays it out of the device's audio jack. You will need headphones or a speaker to hear the audio.

To clone the project, run the following in a terminal:

```
$ git clone https://github.com/resin-io/text2speech.git
```

Once the repo is cloned, cd into the newly created `text2speech` directory and add the resin git endpoint by running the `git remote add` command shown in
the top-right corner of the application page:

```
$ cd text2speech
$ git remote add resin <USERNAME>@git.resin.io:<USERNAME>/<APPNAME>.git
```

Now you can simply run `git push resin master` and push your code up to our servers where they will distribute it to your device(s). If this fails, you may need to force the push by running `git push resin master --force`.

__NOTE:__ On your very first push, git may ask you if you would like to add this host to your list of allowed hosts. Don't worry about this, just type 'yes' and carry on your merry way.

You'll know your code has been successfully compiled and built when our
friendly unicorn mascot appears in your terminal:

<img src="/img/common/pushing/success_unicorn_text2speech.png" width="80%">

The terminal will also say:
```
-----> Image created successfully!
-----> Uploading image..
       Image uploaded successfully!
```
This means your code is safely on our servers and will be downloaded and executed by all the devices you have connected to your application. You may have to wait a little while for the code to start running on your devices. You can see the progress of the device code updates on the device dashboard:

<img src="/img/common/device/device_dashboard_during_update_generic.png" width="80%">

You should now have a friendly talking node.js app and a good base to start building and deploying awesome connected devices.

If node.js isn't your thing, then don't worry, you can use any language you like. Have a look at how to use [dockerfiles][dockerfile] and play around with our python example over [here][python-example].

<!-- ========================== end section =================================   -->

## Further Reading

* For more details on deploying to your devices, see the [deployment guide][deploy].

* If you need more details on managing your devices and applications, check out
  our [device and application management guide][managing_devices_apps].

## Feedback

If you find any issues with the application or get stuck anywhere along the way, please click the small yellow ` ? ` on the bottom right-hand side of the resin.io dashboard and let us know! We are always open to feedback and respond to any issues as soon as we can.

<!-- General Internal (docs) links -->

[deploy]:/deployment/deployment
[projects]:/examples/seed-projects
[managing_devices_apps]:/management/applications
[wifi]:/deployment/network
[supported]:/hardware/devices
[dockerfile]:/deployment/dockerfile

<!-- General External Links -->

[speed_class]:http://en.wikipedia.org/wiki/Sd_card#Speed_class_rating
[signup]:https://dashboard.resin.io/signup
[git]:http://git-scm.com/
[ssh_key]:http://en.wikipedia.org/wiki/Secure_Shell
[pub_key_crypto]:http://en.wikipedia.org/wiki/Public-key_cryptography
[github]:http://github.com/
[github_apis]:https://developer.github.com/v3/
[github_ssh]:https://help.github.com/articles/generating-ssh-keys
[github_ssh_blogpost]:http://resin.io/blog/email-github-public-ssh-key/
[wikihow_format]:http://www.wikihow.com/Format-an-SD-Card
[wikihow]:http://www.wikihow.com/Main-Page
[try-git]:https://www.codeschool.com/courses/try-git
[code-school]:https://www.codeschool.com/

<!-- SD card specific links -->

[fat32]:http://en.wikipedia.org/wiki/Fat32#FAT32
[win32-disk-imager]:http://sourceforge.net/projects/win32diskimager/
[pifiller-download]:http://ivanx.com/raspberrypi/

<!-- Device specific links -->

[rpi]:http://www.raspberrypi.org/
[rpi-b-plus]:http://www.raspberrypi.org/products/model-b-plus/
[rpi-b]:http://www.raspberrypi.org/products/model-b/
[rpi-A-plus]:http://www.raspberrypi.org/products/model-a-plus/
[RPI2-link]:http://www.raspberrypi.org/products/raspberry-pi-2-model-b/
[RPI3-link]:https://www.raspberrypi.org/products/raspberry-pi-3-model-b/
<!-- Example code links -->

[example_app]:https://github.com/resin-io/text2speech
[python-example]:https://github.com/alexandrosm/hello-python
