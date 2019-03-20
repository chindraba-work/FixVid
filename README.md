# FixVid

## Contents

- [Description](#description)
- [Requirements](#requirements)
  - [System Requirements for FixVid](#system-requirements-for-fixvid)
  - [Requirements for FFMPEG](#requirements-for-ffmpeg)
- [Installation](#installation)
- [Usage](#usage)
  - [Synopsis](#synopsis)
  - [Details](#command-details)
- [Copyright and License](#copyright-and-license)


## Description

A [Bash](https://www.gnu.org/software/bash/) script for processing an existing video file, and possibly additional soundtrack or subtitles files, and remuxing the resulting streams into a new video file.

Primary features are the abilty to adjust the video and audio to meet the user's needs when the original version does not.

- Rescale the video to a different size
- Adjust the video dimensions with cropping or letter-boxing
- Change the video codec
- Downmix surround sound audio to a stereo track
- Raise or lower the volume of the audio
- Adjust the dynamic range of the audio

Through the inclusion of other source files, it is also possible to add multiple versions of the subtitles and audio streams. Subtitles added could be for additional languages, or hearing-impaired versions. Other language soundtracks can also be added.

The program uses [FFMPEG](https://www.ffmpeg.org/) for all of the processing and any video codec available on the user's system can be used in the reading and writing of the video.

The audio can be created in AC3 or AAC codecs, or both. In addition, with surround sound sources, the DTS encoding is supported.

The program also includes the ability to create audio streams which are not intended to be included in the remuxed video. These files can be saved separately for use as external audio files in video players which support that option. The same applies to subtitles originally in SubRip or SubStation Alpha formats.

All of the audio and subtitles files created, as well as the remuxed video files, are able to be included in the list of files which are renamed and placed in a named subdirectory, ready for archiving, sharing, or adding to the user's library.

Through the configuration file settings the majority of FFMPEG's video and audio processing features are available, including complex filter graphs if needed.

[TOP](#contents)

## Requirements

### System Requirements for FixVid

- GNU/Linux, or similar system with the same support and capabilities
- File system which supports hard links (`link` command)
- FFMPEG, v3.4, or better `ffmpeg -version | grep version`
- GNU Bash, v4.3, or better `bash --version | grep version`

_[The program is written in Bash script and uses "Bashisms" which limit its operation in most other Unix shells.]_

### Requirements for FFMPEG

- AAC codec encoder `ffmpeg -encoders | grep aac`
- AC3 codec encoder `ffmpeg -encoders | grep ac3`
- Matroska format muxing support `ffmpeg -formats | grep Matroska`
- Advanced Substation Alpha codec encoder `ffmpeg -encoders | grep ASS`
- Encoders for the video codec of choice
- Decoders for the video and audio codecs contained in the source file(s)
- (optional) DTS codec encoder `ffmpeg -encoders | grep DTS`
- (optional for use of Nvidia encoders) an Nvidia graphics card with encoder support

The program itself only requires FFMPEG as an external resource. There are no other required libraries or external binaries needed.

[TOP](#contents)

## Installation

Without the need to link to any system libraries, or install any, the program can be "installed" anywhere, including the user's home directory. A simple option is to place the FixVid-v1.0.0 folder in the user's `bin` directory and create a symlink to the `fixvid` file therein, in the user's `bin` directory. Executing the following commands will result in that arrangement. (With the file names modified for the version downloaded, of course.)

1. Change to the user's `bin` directory

```
$ cd ~/bin
```

2. Download the source file, and signature files

```
$ curl -o FixVid-1.0.0.tar.gz -L https://github.com/chindraba-work/FixVid/archive/v1.0.0.tar.gz
$ curl -o FixVid-1.0.0.tar.gz.sig -L https://github.com/chindraba-work/FixVid/releases/download/v1.0.0/FixVid-1.0.0.tar.gz.sig
```

3. Verify the download against the signature

```
$ gpg --verify FixVid-1.0.0.tar.gz.sig
```

4. Unpack the download

```
$ tar xzf v1.0.0.tar.gz
```

5. Create the symbolic link

```
$ ln -s FixVid-1.0.0/fixvid fixvid
```

The `fixvid` file should already have the executable bit set, so nothing else needs to be done.

In addition, using this method, the symlink need not be called `fixvid`. It could as easily be called `remuxer`, or `my_fancy_fixer`, and then be executed using that name. The system will adapt itself to the new name, including in all the help and usage output.

[TOP](#contents)

---

## Usage

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
init        Scan source candidates, create the fixvid.conf and fixvid.scan files
mix         Create the surround sound audio stream(s)
normalize   Low-level command to normalize the surround sound channels
probe       Show filtered results for the streams in a file
remux       Collect the created streams into a new video file using the given video stream
stereo      Create the stereo audio stream(s)
titles      Extract/process the subtitle stream(s)
version     Display the version number and exit
video       Create the bare video stream(s)
```

[TOP](#contents)

### Command Details

For usage details, see the [USAGE.md](https://github.com/chindraba-work/FixVid/blob/master/USAGE.md) file.

[TOP](#contents)

## Configuration

The `fixvid.conf` file is heavily commented and has all of the available settings listed in it. There is also a file, `fixvid.conf-minimal`, which has the basic settings in it without comments and without listing multiple group settings. With familiarity the `fixvid.conf` file can be removed, and the `fixvid.conf-minimal` renamed in its place.

[TOP](#contents)

# Copyright and License

    Copyright © 2018, 2019 Chindraba (Ronald Lamoreaux)
                           <fixvid@chindraba.work>
    - All Rights Reserved

    FixVid is free software; you can redistribute it and/or
    modify it under the terms of the GNU General Public License,
    version 2 only, as published by the Free Software Foundation.

    FixVid is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program; if not, write to the
          Free Software Foundation, Inc.
          51 Franklin Street
          Fifth Floor
          Boston, MA  02110-1301
          USA.

[TOP](#contents)
