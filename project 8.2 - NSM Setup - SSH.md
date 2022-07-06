# SSH Setup

**On the server:**
1. sudo apt install openssh-server
2. mkdir ~/.ssh/
3. touch authorized_keys
4. sudo chmod 600 authorized_keys
5. ssh-keygen -t ~/.ssh -t rsa -b 4096
6. ssh-copy-id -i ~/.ssh/id_rsa.pub [client username]@[client IP]

**On a remote machine:**
1. ssh [server IP]@[server IP]

**Helpful Documentation:**

- Ubuntu SSH Keys: [https://help.ubuntu.com/community/SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)
- Generate SSH Key: [https://www.ssh.com/academy/ssh/keygen](https://www.ssh.com/academy/ssh/keygen)
