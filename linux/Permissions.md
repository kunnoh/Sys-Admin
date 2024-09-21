## SUID SGID
Create file.
```sh
touch sfile
```

Check attributes.
```sh
ls -l sfile
```

output.
```sh
ls -l sfile    
-rw-r--r-- 1 kun kun 0 Sep 19 16:37 sfile
```

Set `suid`.
```sh
chmod 4664 sfile
```
- 4 - represents suid number.