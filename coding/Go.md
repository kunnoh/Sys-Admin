## Installation
1. download zip. using `wget` download with resume ability using `-c` or `--continue` command.
```sh
wget -c https://go.dev/dl/go1.21.3.linux-amd64.tar.gz
```

2. remove existing go binaries, extract new zip and save to `/usr/local`
```sh
 rm -rf /usr/local/go && tar -C /usr/local -xzf go1.21.3.linux-amd64.tar.gz
```

3. Add /usr/local/go/bin to the `PATH` environment variable
```sh
export PATH=$PATH:/usr/local/go/bin
```

4. check version
```sh
 go version
```


##### variables


##### pointers

