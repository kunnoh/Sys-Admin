# Socket Statistics
The `ss` command stands for "socket statistics." It is a utility in Linux used to display detailed information about network socket connections, similar to the older netstat command but with more capabilities and better performance.

Why `ss`?:
- highly versatile and provides extensive options to monitor and troubleshoot network connections.
- combine different options, you can get the exact information you need for various network administrator


## Check open sockets

```sh
sudo ss -tulnp
```

*Options:*
- `-t`: Show TCP sockets.
- `-u`: Show UDP sockets.
- `-l`: Show only listening sockets.
- `-n`: Show numerical addresses instead of resolving hostnames.
- `-p`: Show the process using the socket.
- ``: 
- `-m`: Memory usage.

Display All TCP and UDP Connections:
```sh
sudo ss -atun
```

Display Established TCP Connections:
```sh
sudo ss -tn state established
```

Display Detailed Information:
```sh
sudo ss -taunp
```

Display IPv4 Connections:
```sh
sudo ss -4
```

Display IPv6 Connections:
```sh
sudo ss -6
```

Display Memory Usage:
```sh
sudo ss -taum
```

Display Internal TCP Information:
```sh
sudo ss -ti
```

Filter by State (e.g., listening):
```sh
sudo ss -tan state listening
```

Display UNIX Domain Sockets:
```sh
sudo ss -xln
```

### Combining Filters:

Display TCP Listening Sockets with Detailed Information:
```sh
sudo ss -tlnp
```

Display All Sockets with Memory and Process Information:
```sh
sudo ss -taump
```

Display UDP Sockets Only:
```sh
sudo ss -un
```

## Reference
1. [Redhat ss](https://www.redhat.com/en/blog/socket-stats-ss)
2. [Net-tools Kali](https://www.kali.org/tools/net-tools/)
