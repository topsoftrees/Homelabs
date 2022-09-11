# SSH Setup

SSH is setup to only allow access from a select number of IP addresses within the same subnet. 

**On the server:**
```bash
sudo apt install openssh-server
mkdir ~/.ssh/
touch authorized_keys
sudo chmod 600 authorized_keys
ssh-keygen -t ~/.ssh -t rsa -b 4096
ssh-copy-id -i ~/.ssh/id_rsa.pub [client username]@[client IP]
```

**On a remote machine:**
```bash
ssh [server IP]@[server IP]
```

## Helpful Documentation:

- Ubuntu SSH Keys: [https://help.ubuntu.com/community/SSH/OpenSSH/Keys](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)
- Generate SSH Key: [https://www.ssh.com/academy/ssh/keygen](https://www.ssh.com/academy/ssh/keygen)
