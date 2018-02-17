---
layout: post
title: FFmpeg Basics
tags: [ffmpeg]
---

# FFmpeg Basics

## Bitrates of video and audio recommended by youtube <sup>[1](#bitrate)</sup>

**Video**

|Type|Standard Frame Rate (24,25,30)|High Frame Rate (48,50,60)|
|---|---|---|
|2160p (4k)|35-45 Mbps|53-68 Mbps|
|1440p (2k)|16 Mbps   |24 Mbps   |
|1080p	   |8 Mbps	  |12 Mbps	 |
|720p	   |5 Mbps	  |7.5 Mbps  |
|480p	   |2.5 Mbps  |4 Mbps	 |
|360p	   |1 Mbps	  |1.5 Mbps  |

**Audio**

|Type|Audio Bitrate|
|---|---|
|Mono  |128 kbps|
|Stereo|384 kbps|
|5.1   |512 kbps|

## Sample commands <sup>[2](#sample)</sup>

Video info check
```
$ ffmpeg -i InputFile.xxx
```

---
Video bitrate: 8000kb/s

Audio bitrate: 192kb/s
```
$ ffmpeg -i InputFile.xxx -b:v 8000k -b:a 192k OutputFile.xxx
```

---
Audio codec: aac

Video codec: mpeg4

Video size: 1280x720
```
$ ffmpeg -i InputFile.xxx -acodec aac -vcodec mpeg4 -s 1280x720 OutputFile.xxx
```




<a name="bitrate">1</a>: [Recommended](https://support.google.com/youtube/answer/1722171?hl=en)

<a name="sample">2</a>: [19 ffmpeg commands](http://www.catswhocode.com/blog/19-ffmpeg-commands-for-all-needs)
