# Socket Statistics
The `ss` command is used to display socket statistics and various network connections.

Key uses of the `ss` command include:
- Viewing current TCP, UDP, and UNIX socket connections.
- Monitoring listening and non-listening sockets.
- Displaying detailed socket information, including memory usage and processes.
- Filtering connections based on protocol, state, and other criteria.


*Basic options*:
- `-t`: Show TCP sockets.
- `-u`: Show UDP sockets.
- `-l`: Show only listening sockets.
- `-n`: Show numerical addresses instead of resolving hostnames.
- `-p`: Show the process using the socket.
- `-a`: Show both listening and non-listening (e.g., established) sockets.
- `-e`: Show detailed socket information.
- `-m`: Show memory usage for sockets.
- `-i`: Show internal TCP information.
- `-4`: Show only IPv4 sockets.
- `-6`: Show only IPv6 sockets.

*Filter options*:
- `-s`: Print summary statistics.
- `-r`: Resolve numeric address/port numbers.
- `-K`: Kill the sockets (requires sudo).


The `ss` command is part of the `iproute2` package, which is the successor to the `net-tools` package that included `netstat`. The `ss` command is considered faster and more efficient than `netstat` because it directly accesses information from the kernel.

## References
- [ss](https://man7.org/linux/man-pages/man8/ss.8.html)
