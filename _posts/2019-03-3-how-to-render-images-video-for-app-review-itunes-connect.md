---
layout: post
title: "How to render images and video for App Review in iTunes Connect"
categories: ios
tags: swift iTunesConnect ios screenshot render app review
fullview: true
---


### 🏞 Render images screenshot App Review

✅ You're able to use **`⌘`** + **`S`**

**Or** you can use **`io`** in **`simctl`**, following these commands:

```bash

xcrun simctl io booted screenshot

```


### 🎞 Render videos 

Steps:

1️⃣ Launch your simulators

2️⃣ Run this commandline:

```bash

xcrun simctl io booted recordVideo input.mp4

```

3️⃣ To stop: **`⌃`** + **`C`**

### ⚙️ Troubleshouting

**📛 App preview dimensions should be: 1920x1080, 1080x1920**

<img width="467" alt="screen shot 2019-03-03 at 11 30 30 am" src="https://user-images.githubusercontent.com/6329656/53690988-caa2a400-3da7-11e9-95be-476051b862b5.png">

You need install **`ffmpeg`**:

```bash

brew install ffmpeg

```

And then:

```bash

ffmpeg -i input.mp4 -acodec copy -crf 12 -vf scale=1080:1920,setsar=1:1 output.mp4

```

**📛 One or nore of your app previews have audio that is not two-channel, no-surround stereo**

<img width="713" alt="screen shot 2019-03-03 at 11 30 12 am" src="https://user-images.githubusercontent.com/6329656/53690989-cbd3d100-3da7-11e9-8402-a944cd3966fd.png">

It means your video is silient, we need to add a sound background for the video by using **iMovie**

You might need this background sound, download the background sound here [sound.mp3](../sounds/sound.mp3)

After download the sound, you need to drag it into **iMovie** and do the **Share** to a file.

**OR** you can use that file with this command:

```bash

ffmpeg -i output.mp4 -i sound.mp3 -c:v copy -c:a aac -strict experimental output_with_sound.mp4

```

### Notes:
✅ A good is video is shorter than 30 seconds

⚠️ iTunes Connect accepts Safari to upload videos.

****

#### 👍 Like & Share please, thanks!!!
