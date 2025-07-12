# Windows 
To reset password:
1. Power PC
2. Turn on PC
3. On login screen, click shutdown, while holding shift press restart. This will restart windows in recovery mode.
4. Goto to `Troubleshoot`. Then go to `Advanced options`. Then go to `Command Prompt`.
5. Backup `Utilman.exe`.
```sh
copy c:\windows\system32\Utilman.exe  c:\windows\system32\Utilman.exebackup
```

6. Copy `cmd.exe` to `Utilman.exe`
```sh
copy c:\windows\system32\cmd.exe  c:\windows\system32\Utilman.exe /y
```

7. Close cmd and click `Continue`.  
8. On login screen, click accessibilty icon. This will open `cmd`.  
9. View users and,  
```sh
net localgroup administrators
```

Set new password.  
```sh
net user <Username> *
```
