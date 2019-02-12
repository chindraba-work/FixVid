# _SmallSite_

## Changelog

### v0.2.1

- Factor out group processing for video and subtitles

### v0.1.2

- Add overlooked changes in command files

### v0.1.1

- Re-enable the AAC encoded files disabled for testing

### v0.1.0

- Thread count has been reduced from 64 to 0 (auto).

- Add parallel processing to audio handlers

- Add the ability to handle 7.1 Surround Sound

- Expanded the `usage` command to provide more information.

- Switch from undescore to hyphen as the separator in filenames.

- Converted the surround sound layout and stereo mixing settings into always set and added the selection of which one to use into the "changeable" section of the config file.

- Added the ability to use dvd_subtitle encoded subtitles from the source file and pass them through to the final generated version of the video.

- Made _movie_ the default source file for all the components in the config file.

- Added the file name of _movie_ (no extension) to the top of the list of files scanned by the `init` command.

- The source files for the audio and subtitles, when they are the same as the video source, can be ignored. If not given, the commands will use the video source file as a fall back.

- Restructure the config file to place the items most needing to be changed, at the end of the file.

