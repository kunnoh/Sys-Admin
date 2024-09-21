#### Check file sizes
To check size of file or directory, use `du`.
```sh
du -sh text.txt
```

Check directory size recursively.
```sh
du -ah text/
```
#### Create archives
`tar` (short for "tape archive") is a command-line utility in Linux and Unix-like operating systems used for creating, archiving, and managing collections of files and directories. It's a common tool for compressing and bundling multiple files into a single archive. Here's a brief explanation:

###### options
- `-c`: Create a new archive.
- `-v`: Verbosely list the files being archived.
- `-f`: Specify the archive file's name.
- `-t`:
- `-r`:
-  `-z`: 

###### Creating an Archive
- You can use `tar` to create an archive by specifying the files and directories you want to include. For example:     
```sh
tar -cvf archive.tar file1.txt file2.txt directory/
```


#### Compression
- Create tar file with name `kun.tar...`.

**bzip2**
Compress
```sh
tar -cjvf kun.tar.bz2 /data/kun/
```

Extract
```sh
tar -xjvf kun.tar.bz2
```

 Use `bzcat` to read the contents of a compressed `.bz2` file without extracting it.
```sh
bzcat kun.tar.bz2
```


**gzip**
Compress
```sh
tar -czvf kun.tar.gz /data/kun/
```

Extract
```sh
tar -xzvf kun.tar.gz
```

 Use `zcat` to read the contents of a compressed `.gz` file without extracting it.
```sh
zcat kun.tar.gz
```

**xz**
Compress
```sh
tar -cJvf kun.tar.xz /data/kun/
```

Extract
```sh
tar -xJvf kun.tar.xz
```

 Use `xzcat` to read the contents of a compressed `.xz` file without extracting it.
```sh
xzcat kun.tar.xz
```


###### Viewing Archive Contents
- You can list the contents of an archive without extracting them using the `-t` option:
```sh
tar -tvf kun.tar.gz
```
   
###### Appending to an Archive
- To add files to an existing archive, use the `-r` option:
```sh
tar -rvf kun.tar.gz newfile.txt
```

###### Combining Multiple Archives
- You can combine multiple archives into a single one using the `--concatenate` option:
```sh
tar --concatenate -f combined.tar archive1.tar archive2.tar
```

###### Extracting Specific Files
- You can extract specific files from an archive using the `-x` option with the `--wildcards` option to match patterns:
```sh
tar -xvf archive.tar --wildcards '*.txt'
```