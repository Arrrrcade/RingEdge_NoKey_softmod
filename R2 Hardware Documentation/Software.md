Software
========

1.  [PCP](/eamuse/sega/software/pcp/)

1.  [libpcp](/eamuse/sega/software/pcp/libpcp.html)

3.  [Device drivers](/eamuse/sega/software/drivers/)

1.  [kbfilter](/eamuse/sega/software/drivers#kbfilter)
2.  [geminifs](/eamuse/sega/software/drivers#geminifs)
3.  [columba](/eamuse/sega/software/drivers#columba)
4.  [mxcmos](/eamuse/sega/software/drivers#mxcmos)
5.  [mxhwreset](/eamuse/sega/software/drivers#mxhwreset)
6.  [mxjvs](/eamuse/sega/software/drivers#mxjvs)
7.  [mxparallel](/eamuse/sega/software/drivers#mxparallel)
8.  [mxsmbus](/eamuse/sega/software/drivers#mxsmbus)
9.  [mxsram](/eamuse/sega/software/drivers#mxsram)
10.  [mxsram\_pci\_isa\_bridge](/eamuse/sega/software/drivers#mxsram_pci_isa_bridge)
11.  [mxsram\_pcmcia](/eamuse/sega/software/drivers#mxsram_pcmcia)
12.  [mxsuperio](/eamuse/sega/software/drivers#mxsuperio)
13.  [mxusbdevice](/eamuse/sega/software/drivers#mxusbdevice)

5.  [Security](/eamuse/sega/software/security/)

1.  [AlphaDVD](/eamuse/sega/software/security/alphadvd.html)

7.  [GrooveMaster.ini](/eamuse/sega/software/groovemaster.html)

Boot Process
------------

The following is a diagram of the Ring\* boot process. Click on any of the programs to learn more about what they do.

The entrypoint, `mxprestartup.exe`, is set as the shell for the `AppUser` user. This is configured in the registry at `HKCU\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\shell`.

C:\\System\\Execute\\mxprestartup.exe C:\\System\\Execute\\mxstartup.exe S:\\mxmaster.exe MASTER\_PROCESS::SysProcessStart s:\\mxkeychip.exe s:\\mxnetwork.exe -p 40104 s:\\mxstorage.exe s:\\mxjvs.exe (not present) s:\\mxinstaller.exe -cmdport 40102 -bindport 40103 MASTER\_PROCESS::FdcProcessStart s:\\mxgcatcher.exe s:\\mxgfetcher.exe s:\\mxgdeliver.exe MASTER\_PROCESS::FirstFgProcessStart APP\_LAUNCHER::CreateThread APP\_LAUNCHER::AppThread (initial launch mode = 6) Launch mode 1 (system error): c:\\System\\Execute\\mxsegaboot.exe -r Launch mode 2 (start game): First found, of: x:\\game.com x:\\game.exe x:\\game.bat If not found: Launch mode 1 Launch mode 3 (start system test menu): c:\\System\\Execute\\mxsegaboot.exe -t -r Launch mode 4 (start game test menu): First found, of: x:\\game.com gametest x:\\game.exe gametest x:\\game.bat gametest If not found: Launch mode 1 Launch mode 5 (???): c:\\System\\Execute\\mxsegaboot.exe -d Launch mode 6 (boot system): c:\\System\\Execute\\mxsegaboot.exe
