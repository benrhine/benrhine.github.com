---
layout: post
title:  "Bedroom Dj 001 - How to properly configure your MacOs audio for streaming with Rekordbox and Blackhole"
date:   2020-12-19
excerpt: "MacOs virtual audio driver configuration"
image: "/images/blackhole-logo.png"
---

## Bedroom Dj 001 - How to properly configure your MacOs audio for streaming with Rekordbox and Blackhole

So you've been practicing your dj skills and are ready to try to start streaming you install what you think you need to complete the task and get ready to go live and everything looks good ... except all your new online fans are posting they cant hear you :( This is an unfortunate reality for us mac users that will even catch the best of us off guard (ie first time I went live I ended up giving up all the streaming stuff from my machine and propped my phone up to watch me). Fortunately I have learned a good bit since then about how to setup streaming software and in solving audio passthrough issues. So you may now be asking "what do I need to do?" to enter this world of audio streaming bliss. To start we are going to install [OBS](https://obsproject.com) which best I can tell is pretty much considered the standard for most streamers followed by [BlackHole](https://github.com/ExistentialAudio/BlackHole).

Being a tech guy I want to make this as fast and as streamlined as possible so we are going to install both of these tools with [Brew](https://brew.sh) (I wont be goig over how to install brew in this post but it is quite simple and there are directions on the brew page). So without further addo open up your terminal and execute the following

`brew install --cask obs`
and
`brew install --cask blackhole-16ch`

Now with both of the above installed we need to configure blackhole as our audio device. If you `CMD-Space` to open spotlight and type `Audio MIDI Setup.app` and open you should see something like the following.

<span class="image center"><img src="{{ "/images/open-midi.png" | absolute_url }}" alt="" /></span>

then

<span class="image center"><img src="{{ "/images/default-audio-devices-blackhole.png" | absolute_url }}" alt="" /></span>

Next click on the PLUS sign and then choose Create Multi-Output Device, make sure everything looks exactly like the image below. Make sure Master device is your built in audio output and the built in is the top check box, if its not just uncheck and then re-check. What this setting config is allowing for telling your mac to send audio out of the speakers as well as send it to blackhole.

<span class="image center"><img src="{{ "/images/blackhole-output-grp.png" | absolute_url }}" alt="" /></span>

Similarly for input click on the PLUS sign and then choose Create Aggregate Device and again make sure everything looks exact.

<span class="image center"><img src="{{ "/images/blackhole-input-grp.png" | absolute_url }}" alt="" /></span>

Once this is done you can click on the labels and rename them to something more appropriate if you like.

Now with blackhole configured it is important to select your new combined audio group as the actual audio output. You can do this by right clicking the audio group and selecting `Use This Device For Sound Output`

##### Note: By selecting this as your primary audio output make sure your volume level is set first as this will disable your volume keys - you will only be able to manually edit the volume level using the software slider while this group is slected as active.

<span class="image center"><img src="{{ "/images/select-audio-device.png" | absolute_url }}" alt="" /></span>

At this point go ahead and open your `Rekordbox` application. Once Rekordbox is open, open your preferences

<span class="image center"><img src="{{ "/images/rb-open-preferences.png" | absolute_url }}" alt="" /></span>

Then navigate to `Audio`, once on the audio tab in the in the audio grouping leave set to DDJ-xxx but make sure the checkbox `Output audio from the computers built-in speakers...` is checked. Then down under `Output channels > Master Output` you should be able to select this and see your new combined audio output group you created in the previous step.

<span class="image center"><img src="{{ "/images/rb-pref-config.png" | absolute_url }}" alt="" /></span>

##### Note: that Rekordbox is somewhat tempermental about this and you may need to open and close the app a few times before it shows up (or maybe I was just struggling, it did show up wihtout to much issue but it wasnt on the first time).

At this point you should have your new virtual audio driver (BlackHole) fully configured and Rekordbox should now be setup to send audio to it. Now how do we test this?

Now its time to open `OBS`, do the same as above `CMD-Space` and type `OBS` and enter. Obs should pop open on your screen. Next select the `Scene` then click the plus (+) in the `Sources` box. Select `Audio Output Capture` as in the screenshot below.

<span class="image center"><img src="{{ "/images/obs-new-audio-capture.png" | absolute_url }}" alt="" /></span>

Give it a name

<span class="image center"><img src="{{ "/images/obs-create-new-audio-capture.png" | absolute_url }}" alt="" /></span>

In the box that opens then select blackhole 16ch

<span class="image center"><img src="{{ "/images/obs-select-blackhole.png" | absolute_url }}" alt="" /></span>

Once added you should see a new volume item added to osb as below

<span class="image center"><img src="{{ "/images/obs-vol.png" | absolute_url }}" alt="" /></span>

To ensure everything is working play some music from Rekordbox and you should see the volume monitor moving around in OSB.

Thats it, you should be good to stream audio directly from your soundcard now, thanks for taking the time to stop by and check out my post.

This post was inspired by the source posts below and are combined here for clarity

[Source 1](https://wearecrossfader.co.uk/blog/stream-from-rekordbox-with-no-soundcard/)
[Source 2](https://wikis.utexas.edu/display/comm/How+to+set+up+BlackHole+Audio+on+a+Mac)




