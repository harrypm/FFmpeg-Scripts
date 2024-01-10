# FFmpeg Scripts


This is my FFmpeg scriptcollection for saving time and automating basic things.

There is`Windows` & `Linux` versions provided.

There is 2 types `Drag Un Drop` & `Batch All` which is ideall for single file and folder level batch tasks.


## Notes 

`-hwaccel auto` Enables automatic levraging of AMD/Intel/Nvida encoding.

`-metadata make= Sony_HVR_Z5E` Sets the device make in the metadata helpful for refrance use.


## Sony HVR-Z5e Handling 


The Sony HVR-Z5e from 2008 is my main camcorder for event use as its a 3CMOS camara with a soild zoom lens its ideal for family and basic events ware a 24-500mm range is needed in 35mm terms. 

My setup however uses an Atmos Ninja Star + iPhone 5S providing rumming time of day timecode LTC over its Aux 3.5mm input this eats channel 3.

This script muxes the audio from channel 3/4 into its own track allowing source files to be easily played back 

Audio_4ch_to_2_track - Takes 4ch Atmos Recording makes 2x 2 track channels.

`ffmpeg.exe -i "%~1" -c:v copy -af "pan=stereo|c0=c0|c1=c1" Shotgun_&_Internal.wav -af "pan=stereo|c0=c2|c1=c3" iPhone5s_LTC_Timecode.wav "%~n1_Fixed_Audio.MOV"`

25p in 50i to 25p - Incase the Atomos Recorder Sets to 50i Interlaced mode (1080i25)

`ffmpeg.exe -hwaccel auto -i "%~1" -c:v prores -profile:v 3 -vf fieldmatch=combmatch=full,bwdif=deint=interlaced -metadata make=Sony_HVR_Z5E -c:a flac -filter_complex "[0]pan=stereo|c0=c0|c1=c1[mic1];[0]pan=stereo|c0=c2|c1=c3[ltc]" -map 0:v -map "[mic1]" -map "[ltc]" -metadata:s:a:0 title="Shotgun_&_Internal" -metadata:s:a:1 title="iPhone5s_LTC_Timecode" -metadata:s:a:0 language=eng "%~n1_Fixed_Audio.mkv"`

50i to 50p - Automatic GPU accelrated BDWIF deinterlacing. (Basic quick use, QTGMC is better I use StaxRip for this)


## Mux MP4 to MKV

I mostly shoot on a Sony A7RIII which makes MP4 video files without a timecode stamp from its internal timecode and the audio is PCM.

As I hate MP4 as a highly headder dependent container I mux this data to a MKV with FLAC compressed audio this saves 100's of GBs and makes my media more robust to handling or file system damages.


## Mux to FLAC 

While I was working on audio I figured I would play with the MKA container format, tbh just stick to `.flac` for 99.9% use cases today for support.

Also MD5 hases the orignal PCM audio stream.


## FFmpeg Checksum 

Creates a sha265 hash for media file.


## V210 to FFV1

Converts Lossless `YUV 10-bit 4:2:2` / PCM 24-bit to Lossless Compressed FFV1 `10-bit 4:2:2` with FLAC compressed audio.

This comes in PAL / NTSC interlaced falvor. 

## FFV1 to V210



## FFV1 to ProRes HQ

Transcodes FFV1 to ProRes HQ with as close as possible flagging.




