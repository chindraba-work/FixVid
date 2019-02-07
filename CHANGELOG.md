# _SmallSite_

## Changelog

### v0.1.0

- Restructure the config file to place the items most needing to be changed, at the end of the file.

- The source files for the audio and subtitles, when they are the same as the video source, can be ignored. If not given, the commands will use the video source file as a fall back.

- Added the file name of _movie_ (no extension) to the top of the list of files scanned by the `init` command.

- Made _movie_ the default source file for all the components in the config file.

- In accordance with `FFMPEG` recommendations, thread count has been reduced from 64 to 16.

- Added the ability to use dvd_subtitle encoded subtitles from the source file and pass them through to the final generated version of the video.

- Converted the surround sound layout and stereo mixing settings into always set and added the selection of which one to use into the "changeable" section of the config file.

- Expanded the `usage` command to provide more information.

- Switch from undescore to hyphen as the separator in filenames.
