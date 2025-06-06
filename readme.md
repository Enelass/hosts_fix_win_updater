## Catastrophic failure
1) My system experienced network freeze, where both explorer.exe and browsers (brave, edge) would freeze or not open at all...
Killing svchost.exe running `%SystemRoot%\system32\svchost.exe -k NetworkService -p` would allow explorer to respond but that process would just re-spawn.

2) Worse  the hosts file, could not be deleted (even as admin) since read by `svchosts`
Killing svchosts like `taskkill /f /im svch*` would expectedly cause a BSOD / Kernal Panic and would free the handle on the file

3) Running  `disable-dnscache-service-win.bat` is not any better...
After running the script my NICs were disabled, and could not be re-enabled.

![Image](https://github.com/user-attachments/assets/4fe2cc59-fe8c-4a9d-9d9f-bbff23b11c1f)

## Reverting the changes:
Reverting the changes implied:

1. Using msconfig to reboot in safe mode (now named)

<img width="266" alt="Image" src="https://github.com/user-attachments/assets/c4a6e3aa-4ef9-4dd0-ba0c-6bec5db01c0b" />

2. Delete hosts, or restoring one of its backup

3. Reverting registry changes
`reg add "HKLM\SYSTEM\CurrentControlSet\services\Dnscache" /v Start /t REG_DWORD /d 2 /f`

4. Reboot
`msconfig`, select "Normal Startup" and restart...


## Possibly a better design for blocking DNS on Windows:
The hosts file wasn't designed to handle such number of entries...
Pi-hole or AdGuard on a container for Windows
or dnscrypto-proxy
