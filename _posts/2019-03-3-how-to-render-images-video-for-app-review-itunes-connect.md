---
layout: post
title: "How to render images and videos for App Review in iTunes Connect"
categories: ios
tags: swift iTunesConnect ios screenshot render app review
fullview: true
---


### 🏞 Render images screenshot App Review with simulator 📱

➡️ You're able to use **`⌘`** + **`S`** with Simulators.

➡️ **Or** you can use **`io`** in **`simctl`**, following these commands:

```bash

xcrun simctl io booted screenshot

```


### 🎞 Render videos with simulator 📱

Steps:

1️⃣ Launch your simulator

2️⃣ Run this

```bash

xcrun simctl io booted recordVideo input.mp4

```

3️⃣ It's recording what you do on the simulator
4️⃣ To stop: **`⌃`** + **`C`**
5️⃣ It created a video within the folder you were runing the command.

### ⚙️ Troubleshouting

**📛 App preview dimensions should be: 1920x1080, 1080x1920**

<img width="467" alt="screen shot 2019-03-03 at 11 30 30 am" src="https://user-images.githubusercontent.com/6329656/53690988-caa2a400-3da7-11e9-95be-476051b862b5.png">

You need install **`ffmpeg`**:

```bash

brew install ffmpeg

```

And then:


For `6.5-inch Display`:

```bash

ffmpeg -i input.mp4 -acodec copy -crf 12 -vf scale=886:1920,setsar=1:1 output.mp4

```

For `5.5-inch Display`:

```bash

ffmpeg -i input.mp4 -acodec copy -crf 12 -vf scale=1080:1920,setsar=1:1 output.mp4

```

**📛 One or nore of your app previews have audio that is not two-channel, no-surround stereo**

<img width="713" alt="screen shot 2019-03-03 at 11 30 12 am" src="https://user-images.githubusercontent.com/6329656/53690989-cbd3d100-3da7-11e9-8402-a944cd3966fd.png">

It means your video is silient, we need to add a sound background for the video by using **iMovie**

You might need this background sound, download the background sound here [sound.mp3](../sounds/sound.mp3) or you can download from [Bensound](https://www.bensound.com/royalty-free-music/corporate-pop)

After download the sound, you need to drag it into **iMovie** and do the **Share** to a file.

- **OR** you can use that file with this command:

```bash

ffmpeg -i output.mp4 -i sound.mp3 -filter_complex " [1:0] apad " -shortest output_with_sound.mp4

```

**📛 The frame rate of one or more of your app previews is too hight.**

<img width="709" alt="screen shot 2019-03-04 at 10 19 14 pm" src="https://user-images.githubusercontent.com/6329656/53742871-37af5a00-3ecc-11e9-8009-11d3c6cc9945.png">

Frame rate should be: **`30fps `**

```bash
ffmpeg -i output.mp4 -filter:v fps=fps=30 output_with_frame_rate.mp4
```

### Notes:
✅ A good is video is shorter than 30 seconds

⚠️ iTunes Connect accepts Safari to upload videos.

****

#### 👍 Like & Share please, thanks!!!
