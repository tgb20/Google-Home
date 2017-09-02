# Google Home Setup

Go to: https://console.cloud.google.com/project

Sign into your google account

Click Create Project at the top

Give it a name, I am using pihome

Open menu in top left and open APIs and Services

Select your project if not already selected in the top

Click Enable Apis and Services

Search for Google Assistant

Click Enable

Click On Credentials

Click on OAuth Consent Screen

Set the 'project name shown to users' as your name from above, pihome

Hit save

Click Create Credentials

OAuth Client ID

Other and call in your name, pihome

Click ok on the pop up

Download your oauth key

Open a new teb and go to: https://myaccount.google.com/activitycontrols

Make sure Web and App Activity, Location History, Device Information, and Voice and Activity are all enabled

# Google Home Install

Open a terminal window

Make sure everything is updated with `sudo apt-get update`

Install python `sudo apt-get install python3-dev python3-venv`

Create our enviroment `python3 -m venv env`

Upgrade pip within the enviroment `env/bin/python -m pip install --upgrade pip setuptools`

Enter our enviroment `source env/bin/activate`

Now we can install Google Home onto our Pi `python -m pip install --upgrade google-assistant-library`

Install the OAuth tool `python -m pip install --upgrade google-auth-oauthlib[tool]`

Run the OAuth Tool `google-oauthlib-tool --client-secrets /home/pi/Downloads/[yourapikey.json] --scope https://www.googleapis.com/auth/assistant-sdk-prototype --save --headless`

Right Click the Link and Copy the URL

Enter that URL in a website and copy the key that shows up

Paste it into the terminal window

We have now finished setting up our Google Home Assistant, now we need to setup out mic and speaker

# Speaker and Mic Setup

First we are going to find the locations of our microphone and speaker

To do this you need to open a new terminal window and type `arecord -l` to list all of your microphone devices, write down the card number and device number.

Now we need to get our speaker information, so type `aplay -l` to list all speakers, write down the card number and device number.

Now we need to tell our Pi to use these devices, type `sudo nano .asoundrc` to edit the sound config file of our Pi, it might be blank or it might have stuff in it. You want to clear it out and replace it with your sound information, like this:

```swift
pcm.!default {
  type asym
  capture.pcm "mic"
  playback.pcm "speaker"
}
pcm.mic {
  type plug
  slave {
    pcm "hw:<card number>,<device number>"
  }
}
pcm.speaker {
  type plug
  slave {
    pcm "hw:<card number>,<device number>"
  }
}
```

One final step is to force the Pi to use the 3.5mm headphone jack.

To do this type `sudo raspi-config` into a terminal window, navigate to Advanced options > Audio and select Force 3.5

Our mic and speaker are now setup

# Testing Google Home

To run Google Assistant go to your env terminal window and type `google-assistant-demo`

To test just say "Hey Google" and your question

# Run on Boot

A Google Home doesn't require you to do anything to get it running, so why should ours? To make it so the google home script runs at boot we have to do a few things.

The first thing we need to do is make our boot script. We can do this with `sudo nano googlehome.sh`

Inside this file we need to do everything we do to get our google home running:
```superscript
#!/bin/sh
echo Starting Google Home...
sleep 2
source env/bin/activate
google-assistant-demo
```
Save with Ctrl+X and then Y

We then need to make sure we can run our script with `chmod +x googlehome.sh`

Now you can run `./googlehome.sh` and it will launch Google Home

We also need to make the Pi load this script on startup. Open the startup file with `sudo nano /home/pi/.config/lxsession/LXDE-pi/autostart`

Right after it says `@xscreensaver -no-splash`, we want to add `@lxterminal -e /home/pi/googlehome.sh`. This runs our script in the terminal when the pi starts. Save with Ctrl+X and then Y

Now you can type `sudo reboot` to see if it works.


