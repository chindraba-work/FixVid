# Purpose

Automate the process of creating a copy of video files such that they meet my requirements.

The targeted requirements (meeting as many as possible) are:

- Stereo sound 'boosted' to new 95% of full-scale,
- Stereo sound 'enhanced' so that there's minimal difference between dialog and sound effects,
- Inclusion of subtitles:
 - Hearing impaired versions when possible,
 - Colorized when practical,
 - Non-HI version included with HI version, when both are available,
- Brightness and contrast adjusted:
 - As compensation for my eyesight,
 - As compensation for HEVC encoding tendancy to 'darken' videos,
- Video size set to 1080p, or the 4:3 equivalent when the source will allow,
 - Reducing to 720p, or the 4:3 equivalent when needed,
 - Rarely, for old, .AVI movies, use even lower sizes,
- Final version packaged in Metroska format,
- Video encoded with HEVC (x265),
- Audio encoded with AAC
- Keep the final file size as small as practical by adjusting the quality factor of the video transcoding.

Additionally, as part of the transcoding, I want to retain the following files:

- The original stereo sound:
 - As provided in the source, or
 - As created by downgrading the surround sound of the source,
- The original surround sound,
- An 'enahanced' and 'boosted' copy of the surround sound,
- Editable copy (Substation Alpha format) of included subtitles,
- Editable copy (Substation Alpha format) of other, non-included, version of the subtitles:
 - Available from BluRay or DVD disks,
 - Found in torrents for the video, and/or
 - Downloaded from subtitle sites (where timing matches the source video).

Originally built using Bash scripting, possible move to Perl as an upgrade.

# Copyright and License

    Copyright © 2018  Ronald Lamoreaux <code@chindraba.work>
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
