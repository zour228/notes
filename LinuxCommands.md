# Filesystem

- List character devices
  ```console
  whsv26@whsv26:~$ sed -n '/^Character/, /^$/ { /^$/ !p }' /proc/devices
  ```

- List block devices
  ```console
  whsv26@whsv26:~$ sed -n '/^Block/, /^$/ { /^$/ !p }' /proc/devices
  ```

- List block devices in tree format with disk/part separation
  ```console
  whsv26@whsv26:~$ lsblk
  ```
  
- List filesystems
  ```console
  whsv26@whsv26:~$ list filesystems
  whsv26@whsv26:~$ sudo fdisk -l
  ```

- Mounting
  ```console
  whsv26@whsv26:~$ DEVICE="/dev/sdb"
  whsv26@whsv26:~$ unmount $DEVICE
  ```

- ISO to flash drive
  ```console
  whsv26@whsv26:~$ ISO="ubuntu.iso" && DEVICE="/dev/sdb"
  whsv26@whsv26:~$ dd bs=4M if=$ISO of=$DEVICE conv=fdatasync status=progress
  ```

- Formatting drive
  ```console
  whsv26@whsv26:~$ FS="bfs|cramfs|ext2|ext3|ext4|f22fs|fat|minix|msdos|ntfs|vfat"
  whsv26@whsv26:~$ mkfs.$FS /dev/sdb
  ```
- Format USB drive
  ```console
  whsv26@whsv26:~$ lsblk
  sdc      8:32   1   7,2G  0 disk 
  └─sdc1   8:33   1   7,2G  0 part /media/whsv26/E0C9-E6FB
  whsv26@whsv26:~$ umount /dev/sdc1
  whsv26@whsv26:~$ mkfs.ext4 /dev/sdc1
  ```

# Package management
- Full delete
  ```console
  whsv26@whsv26:~$ PACKAGE="package-name"
  whsv26@whsv26:~$ apt --purge remove "$PACKAGE*"
  whsv26@whsv26:~$ apt autoremove
  whsv26@whsv26:~$ apt autoclean
  ```

- Search package ```apt search $PACKAGE```

# OS
- Full system info ```dmidecode```
- CPU info ```lscpu```
- RAM info ```dmidecode --type memory | less```
- Running services ```service --status-all```
- Show systemd logs ```journalctl```


- Identify a kernel [@see](https://ubuntu.com/kernel) 
  ```console 
    whsv26@whsv26:~$ cat /proc/version_signature
  ``` 

# Process

- Print process tree
  ```console
  whsv26@whsv26:~$ print process tree
  whsv26@whsv26:~$ pstree -p
  whsv26@whsv26:~$ ps -ejH
  ```

- Watch processes by pattern
  ```console
  whsv26@whsv26:~$ watch 'ps faux | grep $PATTERN'
  ```
  
- Detailed process RAM usage
  ```console
  whsv26@whsv26:~$ sudo pmap $PID
  ```


# SSL
- renew particular cert 
  ```console
  whsv26@whsv26:~$ certbot renew --dry-run --cert-name $CERT_NAME
  ```

- renew all certs and restart nginx 
  ```console
  whsv26@whsv26:~$ certbot renew --post-hook "systemctl restart nginx"
  ```

# Docker
- Kill all running containers
  ```console
  whsv26@whsv26:~$ docker kill $(docker ps -q)
  ```

- Import to mysql container with progress bar
  ```console
  whsv26@whsv26:~$ pv ./dump.sql.gz | gunzip | docker exec -i $CONTAINER_NAME mysql -u$USERNAME -p$PASSWORD --database $DATABASE
  ```

- Psalm language server from docker container
  ```console
  whsv26@whsv26:~$ docker-compose exec -T $SERVICE_NAME ./vendor/bin/psalm-language-server
  ```

# Network

- Check DNS text record ```dig```
- Scan ports ```nmap -A -Pn $IP```
- Check port ```nmap -vv -Pn -p $PORT $IP```
- Show local ports ```sudo netstat -tulpn```
- Resolve host ip and mail exchanger ```host $HOST```

# Archives
- Compress folder ```tar -zcf $ARCHIVE_NAME.tar.gz $FOLDER```
- Decompress folder ```tar -zxf $ARCHIVE_NAME.tar.gz```
- Decompress to stdout ```gzip -dc $ARCHIVE_NAME.gz```

# SSH
- Copy remote file
  ```console
  whsv26@whsv26:~$ scp -P$PORT $USER@$HOST:$REMOTE_PATH $LOCAL_PATH
  ```

- Open ssh-tunnel
  ```console
  whsv26@whsv26:~$ ssh -p $SSH_PORT -i ~/.ssh/$PRIVATE_KEY -N -L $LOCAL_PORT:$REMOTE_IP:$REMOTE_PORT $SSH_USER@$SSH_SERVER_IP
  ```
  
- Pem file from private key
  ```console
  whsv26@whsv26:~$ ssh-keygen -f ~/.ssh/$PRIVATE_KEY -em pem > $PEM_FILE_NAME.pem
  ```

# Databases

#### MySQL

- Dump with condition
  ```console
  whsv26@whsv26:~$ mysqldump --skip-lock-tables -P$PORT -h$HOST -u$USER -p$PASSWORD $DATABASE $TABLE --where="condition" | gzip > backup.sql.gz
  ```

- Dump without exposed credentials
  ```console
  whsv26@whsv26:~$ mysqldump --login-path=local --skip-lock-tables $DATABASE | gzip > backup.sql.gz
  ``` 

- Reload configs ```sudo /etc/init.d/mysql reload```

# Misc

- Set ACL
  ```console
  whsv26@whsv26:~$ setfacl -R -m u:$USER:rwx $DIRECTORY_PATH
  ```
  
- Print all locks
  ```console
  whsv26@whsv26:~$ lslocks
  ```
  
- Find file 
  ```console
  whsv26@whsv26:~$ find $FROM_PATH -name "file*.php" 
  ```
  
- Where the f*ck is it placed 
  ```console
  whsv26@whsv26:~$ readlink -f $SYMLINK
  ```
  
- Что где когда
  ```console
  whsv26@whsv26:~$ whereis
  whsv26@whsv26:~$ which
  ```

- Compile GraphQL schema to html [@dependency](https://github.com/2fd/graphdoc)
  ```console
  whsv26@whsv26:~$ SCHEMA="schema.graphql" && DIR="doc/schema" 
  whsv26@whsv26:~$ graphdoc -s $SCHEMA -o $DIR
  ```
