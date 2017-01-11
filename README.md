# Claymore-XMR-CPU-Miner
Claymore's CryptoNote Windows CPU Miner

Supported coins:
XMR/QCN/BCN/FCN/Aeon/Duck/Dash/OEC/MCN/ORION.

This is POOL version.

This version is for Windows, both x64 and x86. No Linux support planned.

This version uses AES-NI if CPU supports it, but also works with older CPUs that don' support AES-NI.

This miner is free-to-use, however, current developer fee is 2.5%, miner mines 39 rounds for you and 1 round for developer.
If you don't agree with dev fee - don't use this miner, or use "-nofee" option.
Attempts to cheat and remove dev fee will cause a bit slower mining speed (same as "-nofee 1") though miner will show same hashrate.

COMMAND LINE OPTIONS:

-o    pool address. Both HTTP and Stratum protocol are supported. You can specify several "-o" parameters to use several pools, or use "pools.txt" file, or use both. 
   First pool specified via "-o" option is main pool: miner will switch to main pool every 30 minutes.

-u    your wallet address.

-p    password, use "x" as password.

-t    number of threads. "-t 0" - autoselection. Autoselection does not work fine in all cases, so try different values.
   Optimal value depends mostly on L3 cache size. 
   For example, if your CPU has 8 MB of L3 cache (i7 CPUs), use "-t 4". For 6MB L3 cache (i5 CPUs) use "-t 3".

-lowcpu   low CPU usage mode. In this mode only one CPU thread is used but the speed is much higher than in "-t 1" mode.
   This mode is useful for mining in background when minimal CPU usage is required instead of maximal mining speed. 
   Possible values are "-lowcpu 1" or "-lowcpu 2". For example, on i7 4770 CPU "-lowcpu 2" shows about 180 h/s on a single CPU thread.
   This option is available only for CPUs that support AES-NI.

-ee    close miner if no more pools are available in the list. By default, miner tries all pools one by one, after last pool it tries first pool again and so on. 
   Use "-ee 1" to close miner when it tried all pools, so you can restart it from some script and do some additional actions related to internet connectins if necessary.

-dbg   debug log and messages. "-dbg 0" (default) - create log file but don't show debug messages. 
   "-dbg 1" - create log file and show debug messages. "-dbg -1" - no log file and no debug messages.

-nofee: set "1" to cancel my developer fee at all. In this mode some recent optimizations are disabled so mining speed will be slower by about 5%. 
   By enabling this mode, I will lose 100% of my earnings, you will lose only 2.5% of your earnings.
   So you have a choice: "fastest miner" or "completely free miner but a bit slower".
   If you want both "fastest" and "completely free" you should find some other miner that meets your requirements, just don't use this miner instead of claiming that I need 
   to cancel/reduce developer fee, saying that 2.5% developer fee is too much for this miner and so on.

-r   Restart miner mode. "-r 0" (default) - restart miner if something wrong. "-r -1" - disable automatic restarting. -r >0 - restart miner if something 
   wrong or by timer. For example, "-r 60" - restart miner every hour or when some worker thread failed.

SAMPLE USAGE

NsCpuCNMiner64.exe -o stratum+tcp://xmr-eu1.nanopool.org:14444 -u YourWallet.YourPaymentID.YourWorker/YourEmail -p x

Do not forget to specify your wallet address!

FAIL-OVER

Use "-o" option or "pools.txt" file to specify several pools. "pools.txt" file has text format, one address per line. 
If the first character of a line is ";" this line will be ignored. 
After pool address you can also specify login and password, use space as separator, for example:

mro.pool.minergate.com:5556 yourmail@gmail.com !

If login or password are not specified, "-u" and "-p" parameters will be used.

PERFORMANCE

About 280 h/s on i7-4770 ("-t 4")
About 170 h/s on i5-4430 ("-t 3")
32bit version is slower than 64bit version in 1.5-2.0 times, about 190 h/s on i7-4770.

TROUBLESHOOTING

For most cases miner shows detailed error messages with explanations. To achieve maximal mining speed, start miner with admin rights once (miner must show "scfg: 1"), 
it will configure system for optimal performance; then reboot computer to apply changes. For normal work no admin rights or other permissions are required. However, 
if you use Windows UAC and start miner as admin in non-elevated mode miner will not work. Either create normal user and start miner there, or disable UAC.
Miner must show "FAST MODE ENABLED" message if everything is ok.
Sometimes reboot is necessary to clean RAM, otherwise miner can show "not enough memory" error.

Low speed in Windows 8.1 x64:

1. Make sure you are logged as admin. Create shortcut for NsCpuCNMiner64.exe on desktop.
2. Open shortcut properties, and specify command line parameters, for example:
C:\miner\NsCpuCNMiner64.exe -o stratum+tcp://mine.moneropool.org:80 -u 449TGay4WWJPwsXrWZfkMoPtDbJp8xoSzFuyjRt3iaM4bRHdzw4qoDu26FdcGx67BMDS1r2bnp7f5hF 6xdPWWrD3Q3Wf7G6 -p x
3. Press "Advanced" button, check "Run As Administrator". Also disable UAC and reboot (perhaps this step is not ncessary for your configuration).
4. Start shortcut, I get about 290 h/s on stock i7-4770 in Windows 8.1 x64.
