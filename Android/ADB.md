# ADB
## Packages
List all packages.  
```sh
adb shell pm list packages
```

Show user installed packages.  
```sh
adb shell pm list packages -3 | cut -f 2 -d ":"
```

Uninstall package.  
```sh
adb shell pm uninstall -k --user 0 <package_name>
```