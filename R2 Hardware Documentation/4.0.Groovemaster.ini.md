GrooveMaster.ini
================

**Note: This page is specifically regarding maimai, as this is the game I have been focusing my efforts on.** Other games may use a similar format, or they might use a totally different one; I have no idea. This page exists because I was going to make it as a personal notes document but figured it worth making public instead.

Despite the name, `GrooveMaster.ini` is not an ini file. Each line follows the `KEY_NAME value` format. Lines may begin with a hash to indicate a comment. The file is split into four sections, as shown below.

#\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\# maimai DEBUG SETTING
\# デバッグ系コンフィグ設定を記載する
#\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

#====================================================
#描画関係
#====================================================
KEYNAMES values

#====================================================
#システム関係
#====================================================
KEYNAMES values

#====================================================
#GAME ASSINMENT上書き
#====================================================
KEYNAMES values

#====================================================
#デバッグ関係
#====================================================
KEYNAMES values

Config values
-------------

I'll sort these into the categories below later. Maybe.

|Name|Type|Default|Meaning|
|-|-|-|-|
|VIEW\_COURSE|int|0||
|SET\_ICON|int|\-1||
|SET\_TITLE|int|\-1||
|SET\_PLATE|int|\-1||
|SET\_FRAME|int|\-1||
|SET\_CHALLENGE|int|0||
|SET\_OP\_SPEED|int|\-1||
|SET\_OP\_ANSWER|int|\-1||
|SET\_OP\_SE|int|\-1||
|SET\_OP\_BGINFO|int|\-1||
|SET\_OP\_STAR\_ROT|int|\-1||
|SET\_OP\_MIRROR|int|\-1||
|SET\_OP\_MOVIEBRIGHT|int|\-1||
|SET\_JUDGE\_DISP|int|\-1||
|SET\_AIMEID\_1P|int|\-1||
|SET\_AIMEID\_2P|int|\-1||

### Drawing related

|Name|Type|Default|Meaning|
|-|-|-|-|
|ROTATE|int|0|By default two 1920x1080 displays are expected positioned in portrait but set in Windows as landscape, thus the game renders sideways. This setting tells the game to render upright instead.|
|SCREENSHOT|int|0||
|CAPTURE\_X0|int|0||
|CAPTURE\_X1|int|0||
|CAPTURE\_Y0|int|0||
|CAPTURE\_Y1|int0||
|CAPTURE\_W|int|0||
|CAPTURE\_H|int|0||

### System related

|Name|Type|Default|Meaning|
|-|-|-|-|
|DEV|int|0|Enable dev mode. This adds some utility keybindings, and runs a (terrible) touchscreen emulator.|
|NO\_RING|int|0|Indicate that we are not running on Ring\* hardware. This disables the use of all drivers.|
|NO\_JVS|int|0|Disable the use of `\\.\mxjvs`.|
|NO\_SERIAL|int|0|Disable the use of all COM devices (aime readers, LEDs, touchscreen, and the camera).|
|NO\_REBOOT|int|0|Disable the daily 7am reboot.|
|VIRTUAL\_AIME|int|0||
|USB\_DL\_DISABLE|int|0||
|NO\_LINKCHECK|int|0||
|NO\_DELIVER|int|0|Disable OTA updates (via mxgfetcher and mxdeliver).|
|NO\_RESTRICT|int|0||
|1P\_ONLY|int|0|Disable the 2P display. This also disables the top banner, rendering only the main play region of 1P.|
|NO\_WAIT|int|0||
|SET\_FREEPLAY|int|0||
|SET\_TOTAL\_MACHINE|int|\-1||
|SET\_LINK\_ID|int|\-1||
|SET\_TRACKS\_1P|int|\-1||
|SET\_TRACKS\_MULTI|int|\-1||
|SET\_TRACKS\_EVENT|int|\-1||
|SET\_OFFLINE\_MODE|int|\-1||
|SET\_EVENT\_MODE|int|\-1||
|SET\_ADVERTISE\_MODE|int|\-1||
|SET\_ADVERTISE\_SOUND|int|\-1||
|SET\_CAMERA\_POSITION|int|\-1||
|SET\_FRIEND\_TEST|int|0||
|SET\_CLOSE\_HOUR|int|\-1||
|SET\_CLOSE\_MINUTE|int|\-1||
|SET\_ALL\_OPEN|int|\-1||
|SET\_OPEN\_SECRET|int|\-1||
|SET\_OPEN\_EVENT|int|\-1||
|SET\_AIME\_SELECT|int|0||
|SET\_REGION|str|_empty string_||
SET\_CLOCK\_DATE|int|\-1||
|SET\_CLOCK\_BOOST|int|\-1||
|SET\_DRESS\_CODE|int|\-1||
|GO\_CAMERA\_UPLOAD|int|0||
|GO\_COLLECTION|int|0||
|SET\_AUTO\_PLAY|int|\-1||
|SET\_ARAYA\_SPEED|int|\-1||
|LIVE\_COMMENT|int|0||
|SPEAK\_SPEAKER|int|0||
|SET\_AGING|int|\-1||
|QUICK\_BOOT|int|0||

### GAME ASSINMENT overwrite

|Name|Type|Default|Meaning|
|-|-|-|-|
|GO\_RESULT\_1\_1PACV|float|50.0||
|GO\_RESULT\_1\_2PACV|float|50.0||
|GO\_RESULT\_1\_SYNC|float|50.0||
|GO\_RESULT\_2\_1PACV|float|80.0||
|GO\_RESULT\_2\_2PACV|float|80.0||
|GO\_RESULT\_2\_SYNC|float|80.0||
|GO\_RESULT\_3\_1PACV|float|97.0||
|GO\_RESULT\_3\_2PACV|float|97.0||
|GO\_RESULT\_3\_SYNC|float|99.0||
|GO\_RESULT\_4\_1PACV|float|100.0||
|GO\_RESULT\_4\_2PACV|float|100.0||
|GO\_RESULT\_4\_SYNC|float|100.0||

_The following values are all used to force-unlock tracks in the PANDORA BOXX event. Setting them to `8004444900999999` will force-unlock all tracks._

|Name|Type|Default|Meaning|
|-|-|-|-|
|PANDORA\_GREEN\_1P|long|||
|PANDORA\_MAIMAI\_1P|long|||
|PANDORA\_PINK\_1P|long|||
|PANDORA\_MURASAKI\_1P|long|||
|PANDORA\_MILK\_1P|long|||
|PANDORA\_ORANGE\_1P|long|||
|PANDORA\_FINALE\_1P|long|||
|PANDORA\_GREEN\_2P|long|||
|PANDORA\_MAIMAI\_2P|long|||
|PANDORA\_PINK\_2P|long|||
|PANDORA\_MURASAKI\_2P|long|||
|PANDORA\_MILK\_2P|long|||
|PANDORA\_ORANGE\_2P|long|||
|PANDORA\_FINALE\_2P|long|||

### Debug related

|Name|Type|Default|Meaning|
|-|-|-|-|
|SET\_DEBUG\_MODE|int|\-1||
|DEBUG\_MENU|int|\-1||
|DEBUG\_CARD|int|\-1||
|DEV\_STRING|int|\-1||
|DISP\_NETSTAT|int|\-1||
|PERFORMANCE\_POS|int|\-1||
|RESOURCE\_TEXT|int|\-1||
|ITEM\_VIEW|int|\-1||
|HOME\_RANKER\_USERID|str|0:0:0||
|HOME\_RANKER\_3RD\_RATEX100|int|\-1||
|CTRACK\_DAYS|int|0||
|SUPERVISION|int|0||
|OLD\_SERVER\_TRANS\_DISABLE|int|0||
|DEBUG\_COL\_PLAYCOUNT|int|\-1||
|EVENT\_INFO\_CHECK|int|0||
|STRESS\_CHECK|int|0||
|PREV\_FFA\_FLAME|int|10||
|PREV\_SKIP\_FLAME|int|60||
|SET\_SILENT\_MODE|int|0||
|SET\_VIEWER\_PLAY|int|0||
|SET\_SPECIAL\_VIEWER\_PLAY|int|0||
|SET\_NO\_SDT|int|0||
|TIMING\_TAP|int|0||
|TIMING\_HOLDON|int|0||
|TIMING\_HOLDOFF|int|0||
|TIMING\_SLIDE|int|0||
|ACHIEVE\_SLIDE|int|0||
|DISPLAY\_TAP|int|0|

[Previous page](/eamuse/sega/software/security)[sega](/eamuse/sega/)/[software](/eamuse/sega/software/)/groovemaster.html
