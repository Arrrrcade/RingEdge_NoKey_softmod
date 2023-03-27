   Keychip Modding | RingEdge 2    window.dataLayer = window.dataLayer || \[\]; function gtag() { dataLayer.push(arguments); } gtag('js', new Date()); gtag('config', 'G-LG6C6HT317');

[Contents](/eamuse/sega/)

[Intro](/eamuse/sega/intro/)

[Software](/eamuse/sega/software/)

[Hardware](/eamuse/sega/hardware/)

[Manual](/eamuse/sega/manual/)

Keychip Modding
===============

Rather than use a genuine keychip, is it often preferable to mod a system with a keychip emuleator.

The following are the required steps to perform this:

1.  Connect the RingEdge's SSD to another computer. [Instructions for this can be found on the Security page.](/eamuse/sega/software/security.html#ata)
2.  Mount the System partition. This is a TrueCrypt 4 volume located at `C:\Execute\System`. [The password and key can also be found on the Seuciry page](/eamuse/sega/software/security.html#sdrive) .
3.  Replace `mxkeychip.exe` with the emulator binary you were provided. This binary must be renamed to `mxkeychip.exe`.
4.  Create any additional configuration files as required by the specific emulator. These will be emulator-specific.
5.  Dismount everything, and reinsert the SSD into the RingEdge.

Some emulators are provided as a pre-prepared `System` file. In these cases, steps 2, 3, and 4 can be skipped. Instead, simply replace the file located at `C:\Execute\System` with the provided file.

Where to get a keychip emulator
-------------------------------

There are a few emulators floating around, however getting your hands on them is often easier said than done. [micekeychip](https://gitea.tendokyu.moe/Bottersnike/micetools/src/branch/master/src/micetools/micekeychip) is an open-source, but currently incomplete, emulator, however there aren't easily downloadable builds for it.

`mxbadkey` is a good emulator, used with success in many places. The only way I currently know to download it is to contact its author.

`mckeychip_mod` is a tempermental emulator that I wouldn't recommend but have included for completeness. If your system came pre-loaded with an emulator it was probably this one. I do not know who authored it.

[Previous page](/eamuse/sega/manual/errors.html)[sega](/eamuse/sega/)/[manual](/eamuse/sega/manual/)/keychip.html
