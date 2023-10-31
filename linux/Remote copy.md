#### Remote copy
`rsync` or `scp` are used to copy files remotely in `Linux`.

###### Rsync:
Best for: Synchronizing and mirroring directories, especially for backups and large data transfers.
- Advantages:
    - Efficient: Rsync only transfers the differences between files, minimizing data transfer.
    - Resume: It can resume interrupted transfers, ensuring data integrity.
    - Bandwidth-friendly: Rsync can compress data during transfer and uses SSH for secure connections.

- Opinion:
Rsync is excellent for regular backups and efficient data synchronization. It's a preferred choice for large and ongoing data transfers.

###### SCP (Secure Copy):
Best for: Securely copying files from one location to another.
- Advantages:
     - Simplicity: SCP is straightforward to use and part of the SSH protocol.
     - Security: It uses SSH for encryption and authentication, ensuring secure data transfer.
     - One-time transfers: Ideal for quickly copying a single file or a few files.

- Opinion: 
SCP is great for quick, secure file transfers, especially when you don't need continuous synchronization. However, it may not be as efficient for large-scale data syncing as Rsync.

###### example
Copy `/tmp/noma.txt.gpg` file from jump server to App Server 1 at location `/home/web`.

use `scp` to copy files from one host
```sh
scp /tmp/noma.txt.gpg dere@kstapp:/home/web
```

