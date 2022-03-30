---
layout: post
title:  "Bedroom Dj 002 - How to programmatically add cue points to your music collection"
date:   2020-12-19
excerpt: "Automated cue points"
image: "/images/cue-points.png"
---

## Bedroom Dj 002 - How to programmatically add cue points to your music collection

Originally I had intended to write this post quite some time ago and in the time since things have changed somewhat considerably, so I'm going to try to whip this one out. So what am I actually talking about? 

For those of us who dj most of us should know what cue points are but lets start with a quick primer for anyone who may not know. A cue point is essentially a marker at a particular time stamp in the song where you may want to do something. It could be bringing in the next track, applying an effect or marking the rise or fall in a track. There are lots of different ways you can or could use them. By default Rekordbox will try to give you a freebie and add the first cue point to the begining of the track for you. Some of the times it gets it dead on other times not so much. So what does a default track look like with the Rekordbox inserted cue look like vs one with multiple cue points

<span class="image left"><img src="{{ "/images/track-with-default-rb-cue-point.png" | absolute_url }}" alt="" style="width:500px;height:600px;"/></span>

<span class="image right"><img src="{{ "/images/track-with-cue-points.png" | absolute_url }}" alt="" style="width:500px;height:600px;"/></span>

At a quick glance after having seen the above you may be starting to realize that adding cue points to all of your dj collection might be quite the pain. Now there is certainly something to be said for taking the time and hand setting every cue point to just the way you want to use them and if your getting ready for a live gig that is certainly not a bad consideration. With that being said though, most of us just want to play the music so how do we improve our collection with accurate and usable cue points 95% of the time without spending a lifetime to manually set them all? Thats right we are going to programmatically set them using audio fingprinting to the most common places someone might want them.

A few notes at this time. While it would be really nice if Rekordbox supported multiple cue point sets per track but at this time they do not. So assume we run the programmatic cue process but then want to change all the cue points for a particular set. In order to do this you can a) delete all the existing cue points on a given track and manually reset them but they are that way untill you manually do it again or b) add a copy of the track in question to your library and manually set the cue points different on the second track. I know this causes collection bloat but at this time its what is supported. Next to my statement at the top about how things have changed since I considered writing this post. When I first thought about writing this I was on Rekordbox v5 and using an additional tool called Mixed in Key v9. Neither one of these supported programmatic cue setting so I discovered a very useful tool called [Reck](https://atgr-production-team.sellfy.store/p/ma5h/). But now I have since upgraded to Rekordbox v6 and [Mixed in Key v10](https://mixedinkey.com) and Mixed in Key v10 fully supports exporting its fingerprinted cue points to Rekordbox.

So how does this actually work? A bit of a technical deep dive here is that Rekordbox keeps all of your track info, time stamps, cue points, notes, key, bpm, ect in an xml file. What this translates to is if you open your `rekordbox.xml` (which probably lives in your Documents folder) is that if you can pick out the pattern of how cue points are set on a track in the file you could in theory manually edit this file and add cue points to your tracks that way. I've included a track example from the xml below and the element `POSITION_MARK` is marking a cue point.

```
<TRACK TrackID="248143132" Name="In Euphoria We Rise (Progressive Mix)"
           Artist="Aurosonic &amp; Stine Grove" Composer="" Album="" Grouping=""
           Genre="Trance / Progressive" Kind="AIFF File" Size="91575354"
           TotalTime="518" DiscNumber="0" TrackNumber="0" Year="2020" AverageBpm="125.00"
           DateAdded="2021-09-04" BitRate="1411" SampleRate="44100" Comments="1A - Energy 6"
           PlayCount="1" Rating="0" Location="file://location/song.aiff"
           Remixer="" Tonality="01A" Label="Aurosonic Music (RazNitzanMusic)"
           Mix="">
      <TEMPO Inizio="0.158" Bpm="125.00" Metro="4/4" Battito="3"/>
      <TEMPO Inizio="262.717" Bpm="125.00" Metro="4/4" Battito="2"/>
      <POSITION_MARK Name="Cue 1" Type="0" Start="1.118" Num="-1"/>
      <POSITION_MARK Name="Cue 2" Type="0" Start="31.838" Num="-1"/>
      <POSITION_MARK Name="Cue 3" Type="0" Start="62.558" Num="-1"/>
      <POSITION_MARK Name="Cue 4" Type="0" Start="123.998" Num="-1"/>
      <POSITION_MARK Name="Cue 5" Type="0" Start="231.518" Num="-1"/>
      <POSITION_MARK Name="Cue 6" Type="0" Start="262.238" Num="-1"/>
      <POSITION_MARK Name="Cue 7" Type="0" Start="369.757" Num="-1"/>
      <POSITION_MARK Name="Cue 8" Type="0" Start="385.117" Num="-1"/>
      <POSITION_MARK Name="Cue 1" Type="0" Start="1.118" Num="0" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 2" Type="0" Start="31.838" Num="1" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 3" Type="0" Start="62.558" Num="2" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 4" Type="0" Start="123.998" Num="3" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 5" Type="0" Start="231.518" Num="4" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 6" Type="0" Start="262.238" Num="5" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 7" Type="0" Start="369.757" Num="6" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 8" Type="0" Start="385.117" Num="7" Red="40" Green="226"
                     Blue="20"/>
    </TRACK>
    <TRACK TrackID="187071686" Name="Your Mind (Extended Mix)" Artist="Cosmic Gate"
           Composer="" Album="" Grouping="" Genre="Trance / Progressive"
           Kind="AIFF File" Size="79103950" TotalTime="448" DiscNumber="0"
           TrackNumber="0" Year="2020" AverageBpm="128.00" DateAdded="2021-09-04"
           BitRate="1411" SampleRate="44100" Comments="1A - Energy 7"
           PlayCount="0" Rating="0" Location="file://location/song.aiff"
           Remixer="" Tonality="01A" Label="Wake Your Mind Records" Mix="">
      <TEMPO Inizio="0.097" Bpm="128.00" Metro="4/4" Battito="1"/>
      <POSITION_MARK Name="Cue 1" Type="0" Start="0.097" Num="-1"/>
      <POSITION_MARK Name="Cue 2" Type="0" Start="60.097" Num="-1"/>
      <POSITION_MARK Name="Cue 3" Type="0" Start="105.097" Num="-1"/>
      <POSITION_MARK Name="Cue 4" Type="0" Start="165.097" Num="-1"/>
      <POSITION_MARK Name="Cue 5" Type="0" Start="195.097" Num="-1"/>
      <POSITION_MARK Name="Cue 6" Type="0" Start="240.097" Num="-1"/>
      <POSITION_MARK Name="Cue 7" Type="0" Start="292.597" Num="-1"/>
      <POSITION_MARK Name="Cue 8" Type="0" Start="367.597" Num="-1"/>
      <POSITION_MARK Name="Cue 1" Type="0" Start="0.097" Num="0" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 2" Type="0" Start="60.097" Num="1" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 3" Type="0" Start="105.097" Num="2" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 4" Type="0" Start="165.097" Num="3" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 5" Type="0" Start="195.097" Num="4" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 6" Type="0" Start="240.097" Num="5" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 7" Type="0" Start="292.597" Num="6" Red="40" Green="226"
                     Blue="20"/>
      <POSITION_MARK Name="Cue 8" Type="0" Start="367.597" Num="7" Red="40" Green="226"
                     Blue="20"/>
    </TRACK>
```

DISCLAIMER: I do not recommend trying to manually edit the `rekordbox.xml`. If you do attempt to manually edit it please make sure you have a backup. While it is technically possible to do it is very error prone and you dont want to mess up your dj collection.

### Set Cue Points with ReCK


### Set Cue Points with Mixed in Key









