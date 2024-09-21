**Octal values**
- r (read): 4
- w (write): 2
- x (execute): 1

**Find**
```sh
find [/path/to/directory] [search_parameters]
```

Find filename.
```sh
find /path -type f -name hello.txt
```

Find directory.
```sh
find /path -type d -name hello.txt
```

Find file more than 10mb.
```sh
find /path -size +10M
```

**Options:**
-name - case sensitive.
-iname - case insensitive.
-name "f*" - search word with f character.
-size - file size.
-cmin - last created.
-mmin - last modified.
-mmin 5 - last modified in 5mins.
-mtime 5 - last modified in hours.

**and operator**.
```sh
find -name "f*" -size 512k
```

**or operator**.
```sh
find -name "f*" -o -size 512k
```

**not operator**.
```sh
find -not -name "f*"
```
or
```sh
find \! -name "f*"
```

Find directory or file in `/var/log/` group can write to, but others cannot read or write.
```sh
find /var/log/ \( -type f -o -type d \) -perm -g=w \! -perm -o=r \! -perm -o=w
```


**find with permissions**.
```sh
find -perm 664 # exactly this permissions
```
```sh
find -perm -664 # at least 664 permissions
```
```sh
find -perm /664 # any of these permissions
```
