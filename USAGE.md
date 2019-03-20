# FixVid

## Usage

- [Synopsis](#synopsis)
- [Command Details](#command-details)
  - [audio Command](#audio-command)
  - [build Command](#build-command)
  - [channels Command](#channels-command)
  - [dts Command](#dts-command)
  - [finalize Command](#finalize-command)
  - [go Command](#go-command)
  - [init Command](#init-command)
  - [mix Command](#mix-command)
  - [normalize Command](#normalize-command)
  - [probe Command](#probe-command)
  - [remux Command](#remux-command)
  - [stereo Command](#stereo-command)
  - [titles Command](#titles-command)
  - [video Command](#video-command)
  - [&lt;target&gt; Parameter](#target-parameter)
  - [&lt;force&gt; Parameter](#force-parameter)
- [Notes](#notes)

### Synopsis

The basic usage is:
```
$ fixvid <command> [<target> [<force>]]
```
Where `<target>` is the settings group number of the stream to process and `<force>` can be any non-blank argument to force fixvid to do processing it normally would not. If `<target>` is not given or is zero, fixvid will attempt to process all of the settings groups of the appropriate type. The `<force>` argument is not available for all commands.

The available commands are:

```
audio       Create all selected versions of the audio stream(s)
build       Execute the "audio" "titles" and "remux" commands in sequence
channels    Low-level command to extract the surround sound channels
dts         Create the DTS encoded audio stream(s)
final       Copy all created and properly tagged files to the final directory
go          Make and copy all the streams and files indicated in the config file
help        Display general help, or help on the given command or topic
init        [fast] Scan source candidates, create the fixvid.conf and fixvid.scan files
mix         Create the surround sound audio stream(s)
normalize   Low-level command to normalize the surround sound channels
probe       Show filtered results for the streams in a file
remux       Collect the created streams into a new video file using the given video stream
stereo      Create the stereo audio stream(s)
titles      Extract/process the subtitle stream(s)
version     Display the version number and exit
video       Create the bare video stream(s)
```

[TOP](#usage)

### Command Details

#### `audio` Command

```
a [<target>]
audio [<target>]
```

A one-shot command to create all the audio files associated with the `<target>` audio group. Combines the commands `stereo`, `mix`, and `dts` into a single command. In all cases the `<force>` parameter is ignored. May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `build` Command

```
b [<target>]
build [<target>]
```

A one-shot command to create all the files indicated in the fixvid.conf file for the `<targe>` video group as well as all the files indicated for all audio groups and all subtitle groups. It is the equivalent of using the `titles`, `audio`, and `remux` commands in order. In all cases the `<force>` parameter is ignored. May process one video group, or all video groups at once. See `<target>` below.

[TOP](#usage)

#### `channels` Command

```
channels [<target> [<force>]]
```

Create a separate WAV file for each channel of a surround sound stream. This is a low-level command and is normally called as needed by the `mix` command. May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `dts` Command

```
dts [<target> [<force>]]
```

Create DTS formatted surround sound files from the AC3 or AAC files previously created by the `mix` command. Will call the `mix` command if the needed input files are missing.  May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `finalize` Command

```
f
final
finalize
```

Create a properly named subdirectory in the working directory and copy (`link`) all created files which are marked as `_save_` in the `fixvid.conf` file into that directory using the `final_files` setting to create a standard base name, and uses a consistent name for all file types. Will not create missing files, failing silently for each missing file. Ignores both the `<target>` and `<force>` parameters as irrelavent, since all existing and qualifying files are linked and none are created.

[TOP](#usage)

#### `go` Command

```
g [<target>]
go [<target>]
```

A one-shot command to make all audio and subtitle streams, remux the `<target>` video and copy the proper files into the finalized subdirectory. Equivalent to running the `build` and `finalize` commands in sequence. Will process all subtitle groups and all audio groups, but only the `<target>` video group. May process one video group, or all video groups at once. See `<target>` below.

[TOP](#usage)

#### `init` Command

```
init [fast]
```

Create the generic `fixvid.conf` file in the directory and scan for all "mp4", "mkv", "avi", "aac", "ac3", "wav", and  "mp3" files in the current directory and provide a filtered output from ffprobe for each. The scan results are displayed, and recorded in the `fixvid.scan` file. Included in the scan is a file named "movie" without any file type or extension. This file may be any of FFMPEG supported type. This file, if present, will be the first one scanned, and listed at the top of the scan report.

If the `[fast]` parameter is non-blank, a minimalistic version of the `fixvid.conf` file will be created. There are no comments to explain the values, and only one settings group for audio, video and subtitles streams is included. More groups can still be added, but it will need to be completely typed by the user.

[TOP](#usage)

#### `mix` Command

```
m [<target> [<force>]]
mix [<target> [<force>]]
remix [<target> [<force>]]
```

Process the surround sound stream `<target>` according to the settings and requirements in the `fixvid.conf` file. Any combination of the volume levels (original and normalized) and the codecs (AC3 and AAC) are possible. Any combination that is marked as `_include_`, `_save_`, or `_make_` will be created. When the audio group indicated by `<target>` is given as a 2-channel source, `mix` will silently fail. If any normalized versions are required, the original source will be split into individual WAV files, normalized versions of each will also be in WAV files, and the results will be remixed into a file, or files, with the required codec(s). Will automatically call the low-level commands to create the supporting files if needed. Does not create the DTS encoded files, which are created by the `dts [<target>]` command. May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `normalize` Command

```
norm [<target> [<force>]]
normalize [<target> [<force>]]
```

Create normalized versions of each of the separate channels made by the `channels` command. Will call the `channels` command if any of the original files are missing. Will only make normalized versions for channels which do not have one made previously. May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `probe` Command

```
probe <file>
```

Run `ffprobe` on the listed file and filter the results, showing the lines pertinent to the file's duration, chapter list, and each stream located in the file.

[TOP](#usage)

#### `remux` Command

```
r [<target>]
remux [<target>]
rebuild [<target>]
```

Create video files from the files and streams identified in the `fixvid.conf` file. Will call the appropriate commands to build any of the missing streams. Can be used as a one-shot command to completely run through all the needed commands to create each of the streams identified in the `fixvid.conf` file as being included in the final video file. If used as a one-shot, it is possible that some of the streams which the `fixvid config` file directs to be made, but not included in the final file, will not be created. This command will only require the commands to create the versions of the streams which are flagged as being included. The command may create other streams as part of the process, but there is no certainty that all the streams flagged to be made will be created. This command, and any which it calls, will ignore the `<force>` parameter.  May process one video group, or all video groups at once. See `<target>` below.

[TOP](#usage)

#### `stereo` Command

```
stereo [<target> [<force>]]
```

Extract the stereo sound track from the file and stream identified in the `fixvid.conf` file as the stereo source. Applies the enhancements to the stereo sound track and creates an AAC and/or an AC3 file of the original and/or the enhanced tracks. May process one audio group, or all audio groups at once. See `<target>` below.

[TOP](#usage)

#### `titles` Command

```
t [<target> [<force>]]
titles [<target> [<force>]]
subtitles [<target> [<force>]]
```

Create subtitle files from the files and streams identified in the fixvid.conf file. May process one subtitle group, or all subtitle groups at once. See `<target>` below.

[TOP](#usage)

#### `video` Command

```
v [<target> [<force>]]
video [<target> [<force>]]
```

Extract the video from the file and stream identified in the fixvid.conf file. May process one video group, or all video groups at once. See `<target>` below.

[TOP](#usage)

#### `<target>` Parameter

```
<target>
```

Many of the commands can make multiple versions of the streams if they are listed in the config file. Each version has its own group of settings. All audio settings are in `audio_<param>_#`, subtitles settings are in `titles_<param>_#` and video settings are in `video_<param>_#`. For subtitles and audio streams the `_stream_#` parameter is used to determine if that group of settings is to be used, while for video streams the `_codec_#` parameter is used to indicate an applicable group of settings. Each type of stream uses its own sequence of numbers, all begin with 1, and the list must be sequential, with no gaps. If group 2 is not enabled, any other groups, from 3 and above, will not be used, even if they are defined.

Commands with the optional argument of `[<target>]` accept a number for the group of settings to process. If the number is a valid choice, as determined above, then only that group will be processed, while all others will be ignored. If the number is not supplied, or is zero, all groups will be processed in one run of the command. If the number supplied is not in the range of valid numbers, the command will terminate without processing any stream.

As the standard for video files is to only have one primary stream for the video, with the possibility of multiple streams for the audio and subtitles, each group of settings for video streams also indicates a version of the final video file(s) to create. Aside from each having one version of the video, all the final video files will have the same set of audio and subtitle streams.

[TOP](#usage)

#### `<force>` Parameter

```
<force>
```

The `<force>` parameter is available on some of the commands (`channels`, `dts`, `mix`, `normalize`, `stereo`, `titles`, and `video`) where the command will ignore existing, previously created, files and make every possible combination of volume level and codec which can be made with that command.

The `<force>` parameter is not available on the commands which perform multiple commands (`audio`, `build`, `go`, and `remux`) or the `finalize` command, which always overwrites the files anyway.

With the `titles` and `video` commands, which only create one version of the file anyway, it only has the effect of causing the command to overwrite any previously created file.

## Notes

Once the `fixvid.conf` file has been edited with the information about the video file and the streams to process and include, a sensible process would to first use the `video` command, which is the longest running part of the process. Once the video stream has been extracted, and processed, it can be reviewed to determine if changes are needed to the parameters to make it fit the desired file size, or have the quality desired. If the new version does not meet with approval, adjust the settings, delete the file, and re-run the `video` command.

While the video stream is being processed is a good time to review and edit the subtitles, if they are in a text-based format. The other option is to get a fresh cup of coffee or soda and read a chapter or two in your favorite book. Video processing is still the bottleneck, even with a good Nvidia GPU helping out.

Once the video meet with approval, repeat the same process with the audio, using either the `stereo` command or the `channels` command as appropriate. For testing purposes it is most efficient to only create the AC3 encoded version until it also meets with approval. If other codecs, AAC or DTS, are desired, they can be set in the `fixvid.conf` file before the final run.

With the audio and video meeting the desired quality and/or sizes, and the subtitles edited if needed, execute the `go` command, which will create any of the missing audio files (in case you tested with AC3 but want to package the video with AAC or DTS). Also created will be any Substation Alpha subtitles files that have been set in the `fixvid.conf` file. Then the whole is remuxed into the new video file and the versions marked for saving will be copied into the newly created subdirectory. The remuxing process only copies the created streams, so it is rather quick. The creation of additional audio streams will be the slow part if they are used. Once a final review of the freshly created video is done, the subdirectory can be moved to where you want to keep it, and the directory containing the working files can be deleted en-mass. Do make sure that the move operation has completed prior to deleting the directory however, as it can take a while to move subdirectory if you are crossing mount points.

[TOP](#usage)
