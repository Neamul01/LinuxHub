# Install and Configure SSH on Ubuntu 22.04

**Table of Contents**

- [SSH (Secure Shell) Server!](#ssh-secure-shell-server)
- [SSH Architecture](#ssh-architecture)
- [Steps 1: Install SSH on the machine (Ubuntu)](<#steps-1--install-ssh-on-the-machine-(Ubuntu)>)
- [Steps 2: Start and Enable SSH Service](#steps-2-start-and-enable-ssh-service)
- [Steps 3: Configure custom SSH port](#steps-3-configure-custom-ssh-port)
- [Steps 4: Allowing SSH through the firewall](#steps-4-allowing-ssh-through-the-firewall)
- [Steps 5: Connecting to the remote system from your local machine](#steps-5-connecting-to-the-remote-system-from-your-local-machine)
- [Steps 6: Enable,Disable or Status check](#steps-6-enabledisable-or-status-check)

## SSH (Secure Shell) Server!

SSH is a network protocol that provides a secure connection between a client and a server. All communication is encrypted, preventing theft of data transmitted over the network and other remote network attacks.
Commonly used in UNIX-like systems, SSH servers authenticate users and ensure secure communication.

## SSH Architecture

SSH relies on the public-key cryptography to authenticate the remote system and allow it to authenticate the user trying to connect on it. SSH works on three hierarchical layers.

<p align="center">
  <img src="./image/ssh_connection_init.png" alt="SSH Connection Initialization" title="SSH Connection Initialization" height="500" width="850"/>
  <br/>
  Pic: SSH Connection Initialization
</p>

**Here is the detailed steps:**

1. The client initiates a TCP connection to the SSH server on port 22 (default reserved port).
2. The server responds to the client’s connection and they establish a TCP handshake.
3. The server sends its identification string to the client with various informations (protocol version, etc).
4. The client sends its own identification informations to the server using the same format.
5. Once the identification strings have been exchanged, the server sends its public key to the client.
6. If the server is not in the “known_hosts” file of the client, it will prompt the user if they can trust it or not. If yes, the server will be added into the list.
   - If not, this question will prompted everytime
7. The client generates a random session key and encrypt it with the server’s public key.
8. The server decrypts the session key using its private key.
9. The connection between the client and the server is now secured by encrypting each data using the session key.
   [References](https://blog.zedas.fr/posts/linux-explained-8-ssh/)

**Authentication Steps:**

1. The client sends its authentication information to the server. Example here : its public key.
2. The server verifies is the public key is in the “authorized_keys” according to a deductive pattern.
3. If the client’s public key is not authorized, the server rejects it and notify the client.
4. The client tries to authenticate using the username and password.
5. The server validates the provided credentials.
6. The client and servers can now exchange data and commands in a secured encrypted way.

## Steps 1: Install SSH on the machine (Ubuntu)

OpenSSH is not pre-installed on the system, we have to install it manually. To do this, first we have to update the packages to the latest versions and then install `openssh-server` package.

```bash
sudo apt update
sudo apt install openssh-server
```

## Steps 2: Start and Enable SSH Service

First we have to verify that the service is running successfully and then enable service so that it can run automatically at boot time.

```bash
sudo systemctl status ssh
sudo systemctl enable --now ssh
```

## Steps 3: Configure custom SSH port

The default SSH port is 22 and most of the attack scripts check are written around this port only. Changing the default SSH port should add an additional security layer because the number of attacks (coming to port 22) may reduce.

**Before making any changes to configuration files, it's always a good practice to back them up.**

_Backup the file:_

`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config_back`

_Configure the file:_

`sudo vim /etc/ssh/sshd_config`

**Let’s see what steps we can take to secure our SSH server.**

1. Disable empty passwords `PermitEmptyPasswords no`
2. Change default SSH ports `Port 2345`
3. Disable root login via SSH `PermitRootLogin no`
4. Configure idle timeout interval `ClientAliveInterval 300`
5. Disable ssh protocol 1 `Protocol 2`
6. Allow SSH access to selected users only `AllowUsers User1 User2`
7. Disable password based SSH login
