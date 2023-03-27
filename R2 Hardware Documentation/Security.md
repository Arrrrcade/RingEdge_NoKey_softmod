   RingEdge 2    window.dataLayer = window.dataLayer || \[\]; function gtag() { dataLayer.push(arguments); } gtag('js', new Date()); gtag('config', 'G-LG6C6HT317');

[Contents](/eamuse/sega/)

[Intro](/eamuse/sega/intro/)

[Software](/eamuse/sega/software/)

[Hardware](/eamuse/sega/hardware/)

[Manual](/eamuse/sega/manual/)

System Security
===============

The Ring\* series have a number of security measures in place, some easy to bypass, others requiring more work. They are listed here in, roughly, the order in which each layer is applied.

Drive ATA Password
------------------

The SSD contained within the system has an ATA password set. The system BIOS contains a password derivation function that derives the specific password for that drive based on its serial number.

This can be bypassed either by extracting the password used, or by first powering on the Ring\* system with the drive connected, then hotplugging the SATA data cable on the drive while keeping the drive powered.

Why does this work?

The following is the sequence of possible security modes for an ATA drive:

SEC0 Power: off Security: No Power on Freeze HW Reset SEC1 Security: No Frozen: No SEC2 Security: No Frozen: Yes SEC5 Security: Yes Unlocked: Yes Frozen: No SEC4 Security: Yes Unlocked: No Frozen: No SEC6 Security: Yes Unlocked: Yes Frozen: Yes Power on HW reset Unlock Freeze SEC3 Power: off Security: Yes

When the drive has a password set, it initially starts in SEC3. The RingEdge then transitions the drive to SEC5 then SEC6. Importantly, however, as long as power is never lost, the drive will remain in SEC6 mode, even if we connect it to a different system.

This allows us full read-write access to the drive without ever knowing the password!

Windows Password
----------------

It seems silly to mention, but it's worth noting. `AppUser` will automatically log in, but if you need to log back in, or wish to login as `SystemUser`, you'll need the passwords.

`AppUser`'s password is `segahard`.

`SystemUser`'s password is `<6/=U=#tpe!$*3!5`. **NOTE:** if a debugger is attached to `mxprestartup`, `Miflac=Ifme9Jfp0` will be attempted as the password for `SystemUser` instead. This is not the correct password for a production unit.

Finding SystemUser's password

When AppUser logs in, mxprestartup is executed. This binary constructs SystemUser's password then elevates permissions. When no debugger is present, the following steps are performed:

*   Add `153815264b5839090b0d1c1a423c02241633130673071a1e38443912410b47380f213c1d` and `2e0247311e162b666c6640393737724157001f56045b4b4f24333457335a26381f4c3349` together (these are strings contained within the binary).

*   Result: `C:\Windows\System32\wbem\wmitemp.mof`

*   Read `C:\Windows\System32\wbem\wmitemp.mof`.

*   Result: `270a2a053b29042b261d1b22070d140c`

*   Add `152c05381a141f494a48060223260d29` to this value.

*   Result: `<6/=U=#tpe!$*3!5`

If a debugger is present, a similar process is performed:

*   Add `d0b1c034a32243340505a343659517c121f0319583d205c593a690719604846000e062a6` and `63f10541f0c201c0703022f332a13094b542f130c25491a1c280a65440831296e2563590` together, with each byte modulo 255 (not 256!).

*   Result: `34a3c57594e444f47535c53797374756d63323c546279667562737c5d68796f6e2379737`

*   Flip the nibbles of each byte.

*   Result: `C:\WINDOWS\system32\drivers\mxio.sys`

*   Read `C:\WINDOWS\system32\drivers\mxio.sys`.

*   Result: `9160e5c22392918371e43573f2b095b1`

*   Add `43368004f2a34211f4f12120b1b57151` to this value, with each byte modulo 255.

*   Result: `d49666c61636d39466d65693a4660703`

*   Flip the nibbles of each byte.

*   Result: `Miflac=Ifme9Jfp0`

Why is the process for a debugged process more elaborate? I'm not sure.

System binaries encryption
--------------------------

The system binaries, normally located on `S:`, are a TrueCrypt partition. The partition file can be found at `C:\System\Execute\System`. It has password `segahardpassword`, and used the [alternate data stream](https://www.sans.org/blog/alternate-data-streams-overview/) loated at `C:\System\Execute\DLL:SystemKeyFile` as a keyfile.

`C:\System\Execute\DLL:UpdateKeyFile` is also present here, which is used for encryption of update data (but is not utilised in the process of mounting `S:`).

What's an Alternate Data Stream?

An alternate data stream, or ADS for short, is a feature of the NTFS filesystem where additional data can be stored alongside a file or directory. On modern Windows systems, they can be shown using `dir /R`. If you are performing analysis on the system using digital forensics software, which you probably should be, all major packages will show these streams clearly.

![](/eamuse/images/ftk.png)

The alternate data streams, in FTK Imager

The [SANS Institude website](https://www.sans.org/blog/alternate-data-streams-overview/) is a great resource for more information and links.

Finding this information

The decryption of the system partition is the responsibility of `mxstartup`. This file contains pairs of hex strings which sum to produce the paths to the alternate data streams, and the volume password.

### Keyfile downloads

*   [SystemKeyFile](/eamuse/static/keys/SystemKeyFile)
*   [UpdateKeyFile](/eamuse/static/keys/UpdateKeyFile)

Keychip
-------

This is the first point during the boot process where a physical keychip is required in order to continue the system boot process.

With the exception of [`mxkeychip`](/eamuse/sega/software/mx/mxkeychip.html), processes do not directly communicate with the keychip. Instead, they communicate with [`mxkeychip` via a PCP](/eamuse/sega/software/mx/mxkeychip.html). [`mxkeychip`](/eamuse/sega/software/mx/mxkeychip.html) itself is then responsible for communicating with the keychip. These communications are AES encrypted with keys agreed during the initial handshake.

The keychip is responsible for a number of functions:

*   Each keychip is assigned a region, and this region must match the region stored in the Ring\*'s EEPROM.
*   It contains configuration tables for the network setup.
*   It is able to perform basic encryption and decryption, which is used to derive the game encryption key.
*   It includes a large challenge-response table used to authenticate with the specific game.
*   It contains Aime billing information regarding how many credits are allowed on this machine before re-authenticating with network services.
*   It contains a small amount of writable storage used to store trace logs, such as when the system booted or authenticated with network services.

This security layer can be bypassed by replacing the `mxkeychip.exe` binary with a custom binary, eliminating the need to emulate the physical parallel device, and its encryption.

Game data encryption
--------------------

Once the system has verified it is allowed to continue booting, it proceeds to decrypt the game partition. This is done by performing a `keychip.decrypt` request.

The specific key used varies by game. The easiest way to retrieve this key is to, well, ask the keychip for it. We can first start a listener on port `40106` to retrieve the value that the boot process is passing. Following that, we can start the real mxkeychip, and request the same decryption. It will then provide us with the key to be used for the game encryption!

TODO: Some proper digging into the mx binaries to determine exactly where it pulls the encrypted string in the request

The value we have now should be 16 bytes, and is the contents for a keyfile.

Like the `S:` drive, game data is a TrueCrypt partition. Check the [SEGA partition description](/eamuse/sega/misc/partition.html) to determine which partition contains the game data. If in doubt, just try they all until one works :).

Game-keychip handshake
----------------------

There is one final security step present, however I believe this to only be present in some games.

When the game boots, it makes requests to `keychip.ssd.proof` and `keychip.ds.compute`. These are challenge-response queries, which the keychip will internally look up in a large table of possible values.

While we could dump the EEPROM chip located on the keychip, there is no need for this. As the game also needs a copy of the responses to validate against, we can just grab that file, as we have already decrypted the game partition by this point. The file will likely be named `[game ID]_Table.dat`.

I'll flush this out later, but for now, here's the structure of that file:

struct {
    struct {
        char challenge\[7\];
        char responses\[4\]\[20\];
    } ds\[10000\];
    struct {
        char challenge\[16\];
        char response\[16\];
    } ssd\[10000\];
}

The responses are scrambled as described below:

#### DS Scramble:

Output index

0

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

Input index

15

5

13

14

10

1

2

11

16

7

4

18

12

6

3

0

17

8

19

9

#### SSD Scramble:

Output index

0

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

Input index

6

8

4

12

7

13

1

10

2

3

11

14

15

0

5

9

**NOTE: The following information may only be true for MaiMai FiNALE! I have not yet verified this on other games!**  
In dev mode, the game will only ever request a single string as the challenge.

This string is `2CFECBC71CF1E4`, and its corresponding four response pages are (not scrambled):

1.  `ca6ed736401682dd4411a27d2440ac4b478bad8b`
2.  `4ac606302ce5ef51abb3df2dc46c863b3c06aa2c`
3.  `2aaf35b2aba4c6840bdb7bd40ecbce2cca934795`
4.  `2f6d713dabde3c43df818491ab9467ba8ba0fed4`

**NOTE: The following information is super un-verified!**  
Occasionally the game will make a query to the DS table that is not present in the table. In this instance, the keychip responds with the entry at index n, where n is a counter that increments every time a `keychip.ds.compute` query is performed, modulo 100.

It should be noted that if the keychip binary imemdiatly terminates the connection, rather than sending a known-incorrect response (such as if the challenge matches none in the known tables), the game will re-attempt the query, and this query will, with a high likelyhood, be different.

It should additionally be noted that this whole process can be skipped by returning `code=54` rather than a valid response.

[Previous page](/eamuse/sega/software/drivers)[sega](/eamuse/sega/)/[software](/eamuse/sega/software/)/security[Next page](/eamuse/sega/software/groovemaster.html)
