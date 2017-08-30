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
```

One final step is to force the Pi to use the 3.5mm headphone jack.

To do this type `sudo raspi-config` into a terminal window, navigate to Advanced options > Audio and select Force 3.5

Our mic and speaker are now setup

# Testing Google Home

To run Google Assistant go to your env terminal window and type `google-assistant-demo`

To test just say "Hey Google" and your question




