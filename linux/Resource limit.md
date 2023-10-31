#### Resource limit
On our Storage server, we are having some issues where nfsuser user is holding hundred of processes, which is degrading the performance of the server. Therefore, we have a requirement to limit its maximum processes. Please set its maximum process limits as below:

a. soft limit = 1024
b. hard_limit = 2025

1. edit `/etc/security/limits.conf` and add `@nfsuser soft nproc 1024` and `@nfsuser hard nproc 2025`.





