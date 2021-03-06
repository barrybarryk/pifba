This is a slightly tweaked version of PiFBA / FBA2x for the Raspberry Pi 2.

What's changed?
I've enabled CPS3 emulation, at the minute it's just the three Street Fighter III games.

***ONLY THE NOCD VERSIONS WILL WORK***

So that's "sfiiin.zip", "sfiii2n.zip" or "sfiii3n.zip".  And they go in your normal rom folder with no special requirements (like a bios rom).

The zip files for correct versions of the working roms will contain EXACTLY these files, if your rom zip file contains different files it's not the same version and it won't work:-

// --------------------------------------------------------
// Street Fighter III: New Generation (Asia 970204, NO CD)
// --------------------------------------------------------
sfiiin.zip -
	sfiii_asia_nocd.29f400.u2
	10
	30
	31
	40
	41
	50

// ----------------------------------------------------------------
// Street Fighter III 2nd Impact: Giant Attack (Asia 970930, NO CD)
// ----------------------------------------------------------------
sfiii2n.zip -
	sfiii2_asia_nocd.29f400.u2
	10
	20
	30
	31
	40
	41
	50
	51

// -------------------------------------------------------------------------
// Street Fighter III 3rd Strike: Fight for the Future (Japan 990512, NO CD)
// -------------------------------------------------------------------------
sfiii3n.zip -
	sfiii3_japan_nocd.29f400.u2
	10
	20
	30
	31
	40
	41
	50
	51
	60
	61

Why just the 3 Street Figther III games?
Baiscally, because I'm still testing, they're the three games most people are going to want to play and they're the three games the emulation core is optimised towards.  At the minute there's still no sound, but once I get the CPS3 audio working I'll add in the other CPS3 games (there aren't many).

How to compile?
Compile with 'make -f Makefile.PiCPS3 -j4'

At the minute this is only compatible with the Pi 2, simply because with the few attempts I made to load a CPS3 rom in a FBA2X binary without the Cortex-A7 and NEON compile flags, the emulator crashed with a floating point exception.  I'm sure there's a set of compile options that'll allow this to work on a Pi 1, but I'd imagine CPS3 emulation would be too demanding for the Pi 1 models anyway, so that problem is on the back burner until I can get the audio working.  - BarryK



***  ORIGINAL README  ****
=Final Burn Alpha 2x for Raspberry Pi by Squid=

*INTRODUCTION*

PiFBA is Final Burn Alpha 2x for the Raspberry Pi ported by Squid. This emulates old arcade games using MAME based roms for CPS1, CPS2, Neogeo, Toaplan and many other games. It should support most recent versions of the MAME versions of these ROMS but unfortnately there is no definitive version to use. A full list of supported games is in the frontend program.

Some large ROMS may require converting to the '.fba' format before they will work. FBACache_windows.zip is included to do this, which runs on Windows not Linux.

A clrmamepro/romcenter DAT file is included 'fba_029671_clrmame_dat.zip' which supports most of the ROMS that work and can generate compatible ROMS from recent versions of MAME.

A number of games will not work properly, some are so large they appear to run out of memory.

It uses a modified version of the Capex frontend for game selection but you can use your own frontend and simply call the "fba2x" to play the games.

This port supports keyboard and USB joystick input and are configurable. It uses GLES2 graphics API and can run in the Console or under X-Windows. The audio uses the ALSA API and SDL is used for input. It uses a Cyclone 68000 core for performance and a MAME z80 core for compatibility.

Download it from the official Raspberry Pi App Store:
http://store.raspberrypi.com/projects/pifba

Alternatively download it manually from:
http://pifba.googlecode.com/git/piFBA.zip

Web page for news, source, additional information:
http://code.google.com/p/pifba/

(No asking for ROMS or any issues with ROMS)


*CONTROLS*

Keyboard controls:
{{{
'up arrow'       Up direction
'down arrow'     Down direction
'left arrow'     Left direction
'right arrow'    Right direction
'control'        Button 1 (starts a game in the frontend)
'alt'            Button 2
'space'          Button 3
'shift'          Button 4
'z'              Button 5
'x'              Button 6
'return'         Start button (starts a game)
'tab'            Select button (opens config menu in the frontend)
'escape'         Quit
'L+R+SELECT'     Service Mode
}}}

Additionally to quit with the joystick you can press SELECT+START.

The joystick layout defaults to Saitek P380 button layout. 

All controls are configurable by editing the "fba2x.cfg" file. If your joystick uses axis different than 0 and 1 you can change this in the fbax2.cfg file.

The frontend displays the list of ROMS in the following colours:
RED = missing
ORANGE = clone rom detected
YELLOW = parent rom detected or clone rom detected & parent rom detected
GREEN = clone rom detected & parent rom detected 


*INSTALLATION*
{{{
fba2x       -> Game binary
fbacapex    -> Frontend binary
fba2x.cfg   -> Keyboard and joystick configuration
rominfo.fba -> required by the frontend
zipname.fba -> required by the frontend
preview/    -> screenshots (BMP 384x224)
roms/       -> ROMs directory (Put your game ROMs in here)
skin/       -> Frontend skins directory
FBACache_windows.zip -> for converting large ROMS to .fba cache format
fba_029671_clrmame_dat.zip -> clrmamepro DAT file for ROM validation and conversion.
}}}

To run piFBA simple run the "fbacapex" executable. At the command line "./fbacapex".
This runs the GUI frontend. To simply run piFBA without the GUI enter "./fba2x roms/{gamerom}" where "{gamerom}" is the game rom filename to run. A number of options are available for "fba2x", enter "./fba2x" on the command line for a list.

For the frontend screenshots you need to put the relevant screenshot files in the preview subdirectory. These files should be BMP and 384x224 (non-compressed BMP) dimensions.

An output file called "output.log" is generated for each game which may show useful information for problem ROMS.


*Pi CONFIGURATION*

I highly recommend overclocking your Raspberry Pi to gain maximum performance
as PiFBA is very CPU intensive and overclocking will make most games run at full speed.

My overclocking settings which work well, (/boot/config.txt)
{{{
arm_freq=900
sdram_freq=500
}}}

NOTE: Make sure overclocking is actually working by checking "cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor" should be "ondemand". Later kernels appear to set it to "powersave" by default.

If your sound is too quiet then do the following to fix that:
{{{
First get the playback device, type "amixer controls"
This will show the numid for the playback device, probably 3.
Now set the volume, type "amixer cset numid=3 90%".
Then reboot to make it permanent.
}}}

If you're having problems with HDMI audio then it is likely PulseAudio
is causing the issues as it has problems with the ALSA drivers. To fix
this simply remove PulseAudio:
{{{
sudo apt-get --purge remove pulseaudio
sudo apt-get autoremove
}}}

If you're getting a black screen when running in Console mode with
Composite output, try removing/commenting out the "overscan_" parameters from "/boot/config.txt" as follows (using disable_overscan doesn't appear to fix it):
{{{
#overscan_left=16
#overscan_right=16
#overscan_top=16
#overscan_bottom=16
}}}

*GRAPHICS EFFECTS*

Postprocessing can be enabled by setting "DisplayEffect" in fba2x.cfg. In addition you can disable anti-aliasing smoothing ("DisplaySmoothStretch" in fba2x.cfg). For the scanlines filter it is recommended you switch off "DisplaySmoothStretch".

*PORT CREDITS*

  * Ported to and optimised for the Raspberry Pi by Squid (aka Pudding on the Pi forums)

*ORIGINAL CREDITS*

  * Final Burn copyright Team FB Alpha.
  * FBA2X version 7.3 by HeadOverHeels.
  * CAPEX frontend originally by JYCET and Juanvvc
  * FB Alpha is based on Final Burn (formally at http://www.finalburn.com/).
  * Musashi MC68000/MC68010/MC68EC020 core by Karl Stenerud (http://www.mamedev.org/).
  * A68K MC68000 core by Mike Coates & Darren Olafson (http://www.mamedev.org/).
  * YM3812 core by Jarek Burczynski & Tatsuyuki Satoh (http://www.mamedev.org/).
  * YM2151 core by Jarek Burczynski (http://www.mamedev.org/).
  * YMF278B core by R. Belmont & O.Galibert (http://www.mamedev.org/).
  * YM2608/YM2610 cores by Jarek Burczynski & Tatsuyuki Satoh (http://www.mamedev.org/).
  * AY8910/YM2149 core by various authors (http://www.mamedev.org/).
  * M6502 emulation core by Marat (http://fms.komkon.org/EMUL8/).
  * I8039 emulation core by Mirko Buffoni (http://www.mamedev.org/).
  * Z80 emulation core by Dave, modifications by Team FB Alpha (http://www.finalburn.com/).
  * PNG functionality provided by libpng (http://www.libpng.org/) and PNGlib (http://www.madwizard.org/).
  * Zip functionality provided by zlib (http://www.zlib.net/).

*CHANGE LOG*


_October 02, 2013:_
  * Added DPAD (HAT) controls support for joysticks.

_September 01, 2013:_
  * Joystick SELECT+START now quits the frontend. Joystick SELECT now opens the menu and START will start a game.
  * 2nd player controls now configurable for joystick and keyboard.

_August 11, 2013:_
  * Fixed keyboard not working when no joysticks attached

_August 7, 2013:_
  * Fixed sound in some games, e.g. pang3, forgottn
  * All sound now rendered in stereo (mono uses both channels)

_August 5, 2013:_
  * Added two player joystick support
  * Fixed frontend neverending quit with joysticks

_July 30, 2013:_
  * Higher resolution frontend

_July 24, 2013:_
  * Fixed some games
  * Added more Taito games
  * Added "MaintainAspectRatio" option to allow filled screen

_July 21, 2013:_
  * Initial release.
