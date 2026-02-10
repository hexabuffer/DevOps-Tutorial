<p style="text-align:center; font-size: 30px; font-weight: bold">SSH Essentials: Working with SSH Servers, Clients, and Keys</p>

---

## Table of Contents

1. [Introduction](#introduction)
2. [SSH Overview](#ssh-overview)
3. [How SSH Works](#how-ssh-works)
4. [Authentication Methods](#authentication-methods)
5. [Generating and Managing SSH Keys](#generating-and-managing-ssh-keys)
6. [Connecting to Remote Servers](#connecting-to-remote-servers)
7. [SSH Configuration](#ssh-configuration)
8. [SSH Tunneling](#ssh-tunneling)
9. [Troubleshooting](#troubleshooting)
10. [FAQs](#faqs)
11. [Conclusion](#conclusion)

---

## Introduction

[SSH (Secure Shell)](https://www.openssh.org/) is a secure protocol used as the primary method for connecting to and managing remote Linux servers. It provides encrypted access for running commands, transferring files, and forwarding network traffic, making it a foundational tool for system administration and development workflows.

This guide covers both the fundamentals and the practical details required to work effectively with SSH. In addition to basic connection methods, it explains how SSH authentication works, how trust is established using keys, and how to configure SSH securely on both the server and client side. It also includes guidance on tunneling traffic, managing sessions, and diagnosing common connection issues.

### Key Takeaways

- **SSH is the default tool for secure remote access** to Linux servers. It enables encrypted command execution, file transfers, and network tunneling over untrusted networks.

- **SSH relies on a client-server architecture** with explicit responsibilities. The SSH daemon manages incoming connections on the server, while the client controls authentication and session behavior.

- **SSH keys provide stronger security than passwords** and should be the standard. Key-based authentication avoids shared secrets and prevents brute-force and credential reuse attacks.

- **SSH trust is established ahead of time**, not during login. Servers trust public keys placed in `authorized_keys`, and clients prove identity by possessing the matching private key.

- **User authentication and host verification solve different security problems**. SSH keys authenticate users, while host keys protect clients from connecting to impersonated servers.

- **Private key protection is critical to SSH security**. Private keys must remain local, secured with permissions and optional passphrases, and never copied to servers.

- **A secure SSH baseline dramatically reduces exposure**. Disabling password logins, blocking direct root access, limiting attempts, and explicitly allowing users should be standard practice.

- **Client-side SSH configuration improves safety and usability**. The `~/.ssh/config` file simplifies connections, enforces consistent behavior, and reduces human error.

- **SSH tunneling extends SSH beyond remote shells**. Local, remote, and dynamic tunnels allow encrypted access to services and networks that would otherwise be unreachable.

- **Most SSH problems are predictable and diagnosable**. Permission issues, authentication mismatches, service availability, and host verification errors account for the majority of failures.

---

## SSH Overview

The most common way of connecting to a remote Linux server is through SSH. **SSH stands for Secure Shell** and provides a safe and secure way of executing commands, making changes, and configuring services remotely. When you connect through SSH, you log in using an account that exists on the remote server.

---

## How SSH Works

### Client-Server Model

When you connect through SSH, you will be dropped into a **shell session**, which is a text-based interface where you can interact with your server. For the duration of your SSH session, any commands that you type into your local terminal are sent through an encrypted SSH tunnel and executed on your server.

The SSH connection is implemented using a **client-server model**:

- **SSH Daemon (Server)**: The remote machine must be running an SSH daemon, which listens for connections on a specific network port (default: 22), authenticates connection requests, and spawns the appropriate environment if the user provides the correct credentials.

- **SSH Client**: The user's computer must have an SSH client that knows how to communicate using the SSH protocol and can be given information about the remote host to connect to, the username to use, and the credentials that should be passed to authenticate.

---

## Authentication Methods

### Password vs. SSH Key Authentication

Clients generally authenticate either using **passwords** (less secure and not recommended) or **SSH keys** (very secure).

#### Password Authentication
- **Pros**: Encrypted and easy to understand for new users
- **Cons**: Vulnerable to automated brute-force attacks and credential reuse

**Recommendation**: Password authentication should be disabled in favor of SSH key-based authentication.

#### SSH Key Authentication

SSH keys are a **matching set of cryptographic keys** which can be used for authentication:

- **Public Key**: Can be shared freely without concern
- **Private Key**: Must be vigilantly guarded and never exposed to anyone

### How SSH Key Authentication Works

1. **Setup Phase**: 
   - User generates an SSH key pair on their local computer
   - The public key is copied to the server at `~/.ssh/authorized_keys`
   
2. **Authentication Phase**:
   - Client connects and attempts SSH key authentication
   - Server checks the `authorized_keys` file for the corresponding public key
   - Server sends authentication data to the client
   - Client uses the private key to create a digital signature
   - Server verifies the signature using the public key
   - If verification succeeds, the connection is allowed

### How SSH Establishes Trust

**SSH key authentication uses asymmetric cryptography** to establish trust without sharing credentials:

- Trust is created when a server administrator places a public key into the `~/.ssh/authorized_keys` file
- By adding a public key, the server is explicitly configured to trust any client that can demonstrate possession of the matching private key
- During authentication, the server issues a cryptographic challenge that can only be answered correctly by a client that holds the private key
- **The private key never leaves the client system** and is never transmitted over the network
- Because the server stores only public keys, a server compromise does not expose private credentials

### User Authentication vs. Host Verification

SSH uses **two separate trust mechanisms**:

| Mechanism | Purpose | Implementation |
|-----------|---------|----------------|
| **User Authentication** | Verifies that the client is allowed to log in to an account on the server | SSH keys and `authorized_keys` file |
| **Host Verification** | Confirms that the client is connecting to the intended server | Server host keys stored in `~/.ssh/known_hosts` |

These mechanisms operate independently. SSH key authentication confirms **who is logging in**, while host verification protects against connecting to an **unexpected or impersonated server**.

---

## Generating and Managing SSH Keys

### Generating an SSH Key Pair

Generating a new SSH public and private key pair on your local computer is the first step towards authenticating with a remote server without a password.

#### Recommended Algorithms

| Algorithm | Key Size | Recommendation |
|-----------|----------|----------------|
| **Ed25519** | Fixed | **Preferred** - strong security, small key size, good performance |
| **RSA** | 3072-4096 bits | Widely supported, use minimum 3072 bits |
| **ECDSA** | Variable | Supported but less common |

#### Basic Key Generation

To generate an RSA key pair:

```bash
ssh-keygen
```

**Prompts:**

1. **File location**: Press `ENTER` to use default (`~/.ssh/id_rsa`)
2. **Passphrase**: Enter a passphrase for additional security, or press `ENTER` for no passphrase

**Output files:**
- `~/.ssh/id_rsa` - The private key (**DO NOT SHARE THIS FILE!**)
- `~/.ssh/id_rsa.pub` - The public key (can be shared freely)

### Generate SSH Key with Larger Key Size

For enhanced security with RSA keys:

```bash
ssh-keygen -b 4096
```

Most servers support RSA keys up to 4096 bits. Larger key sizes may not be supported by all implementations.

### Changing or Removing a Passphrase

To change or remove the passphrase on an existing private key:

```bash
ssh-keygen -p
```

**Note**: You must know the original passphrase. If lost, you must generate a new key pair.

### Displaying the SSH Key Fingerprint

To view the fingerprint of an SSH key:

```bash
ssh-keygen -l
```

**Output example:**
```
4096 8e:c4:82:47:87:c2:26:4b:68:ff:96:1a:39:62:9e:4e demo@test (RSA)
```

The fingerprint uniquely identifies the key and can be useful for verification.

---

## Copying Your Public Key to a Server

### Method 1: Using ssh-copy-id (Recommended)

If you have password-based SSH access and the `ssh-copy-id` utility:

```bash
ssh-copy-id username@remote_host
```

This command:
1. Prompts for the remote user's password
2. Appends your public key to `~/.ssh/authorized_keys` on the server
3. Sets appropriate permissions

After completion, you can log in without a password:

```bash
ssh username@remote_host
```

### Method 2: Manual Copy

If `ssh-copy-id` is unavailable:

```bash
cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### Method 3: Physical Copy

If you don't have password access:

1. Display your public key:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

2. Copy the output

3. On the remote server, add it to authorized_keys:
   ```bash
   mkdir -p ~/.ssh
   echo "your_public_key_string" >> ~/.ssh/authorized_keys
   chmod 700 ~/.ssh
   chmod 600 ~/.ssh/authorized_keys
   ```

---

## Connecting to Remote Servers

### Basic Connection

**Same username on local and remote:**
```bash
ssh remote_host
```

**Different username:**
```bash
ssh username@remote_host
```

### First-Time Connection

On first connection, you'll see a host verification message:

```
The authenticity of host '111.111.11.111 (111.111.11.111)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)?
```

Type `yes` to accept and add the host to your `known_hosts` file.

### Running a Single Command

Execute a command without opening an interactive session:

```bash
ssh username@remote_host command_to_run
```

### Connecting to Different Port

```bash
ssh -p port_number username@remote_host
```

### Using SSH Agent

If you have a passphrase on your private key, use an SSH agent to avoid repeated password entry:

**Start the agent:**
```bash
eval "$(ssh-agent -s)"
```

**Add your private key:**
```bash
ssh-add
```

**Output:**
```
Enter passphrase for /home/demo/.ssh/id_rsa:
Identity added: /home/demo/.ssh/id_rsa (/home/demo/.ssh/id_rsa)
```

Now you can connect without re-entering the passphrase for the duration of your terminal session.

### SSH Agent Forwarding

To use your SSH credentials on a server you've connected to:

```bash
ssh -A username@remote_host
```

This forwards your SSH credentials, allowing you to connect to additional servers from the remote host without copying your private key.

---

## SSH Configuration

### Server-Side Configuration

#### Secure Baseline Configuration

A secure SSH baseline should:

1. ✅ **Prefer SSH key-based authentication** and phase out password logins
2. ✅ **Restrict who can log in** (limit root access, use AllowUsers/AllowGroups)
3. ✅ **Limit repeated authentication attempts** to reduce brute-force attacks
4. ✅ **Restart SSH service** after configuration changes

#### Configuration File Location

```bash
sudo nano /etc/ssh/sshd_config
```

#### Critical Security Settings

**1. Disable Password Authentication**

```
PasswordAuthentication no
```

**2. Disable Root Login**

```
PermitRootLogin no
```

Or allow root login only with SSH keys:

```
PermitRootLogin without-password
```

**3. Limit Authentication Attempts**

```
MaxAuthTries 3
LoginGraceTime 20
```

**4. Explicitly Control User Access**

Allow specific users:
```
AllowUsers user1 user2
```

Allow specific groups:
```
AllowGroups sshusers
```

Deny specific users:
```
DenyUsers baduser
```

**5. Change Default Port (Optional)**

```
Port 4444
```

**Note**: Changing the port requires updating firewall rules and client configurations.

#### Applying Configuration Changes

After editing `/etc/ssh/sshd_config`:

**Test configuration:**
```bash
sudo sshd -t
```

**Restart SSH service:**

Ubuntu/Debian:
```bash
sudo systemctl restart ssh
```

CentOS/RHEL:
```bash
sudo systemctl restart sshd
```

**⚠️ Important**: Always maintain an active SSH session when modifying SSH configuration to avoid being locked out.

### Client-Side Configuration

#### SSH Config File

Create or edit `~/.ssh/config` on your local machine to define server-specific settings:

```bash
nano ~/.ssh/config
```

#### Basic Configuration Example

```
Host testhost
    HostName your_domain
    Port 4444
    User demo
```

Connect using the alias:
```bash
ssh testhost
```

#### Using Wildcards

```
Host *
    ForwardX11 no
    ServerAliveInterval 60

Host testhost
    HostName your_domain
    ForwardX11 yes
    Port 4444
    User demo
```

**Note**: Later matches override earlier ones, so place general matches at the top.

#### Useful Client Options

**Keep connections alive:**
```
ServerAliveInterval 60
ServerAliveCountMax 3
```

**Disable host checking (not recommended for production):**
```
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
```

**SSH multiplexing (reuse connections):**
```
ControlMaster auto
ControlPath ~/.ssh/sockets/%r@%h-%p
ControlPersist 600
```

---

## SSH Tunneling

SSH tunneling allows you to forward network traffic through an encrypted SSH connection.

### Local Tunneling

**Use case**: Access a service on a remote network from your local machine.

**Syntax:**
```bash
ssh -L local_port:destination_host:destination_port username@ssh_server
```

**Example**: Access a remote web server on port 80 via local port 8888:

```bash
ssh -f -N -L 8888:your_domain:80 username@remote_host
```

Access in browser: `http://127.0.0.1:8888`

**Flags:**
- `-f` - Go to background before command execution
- `-N` - Do not execute a remote command (port forwarding only)
- `-L` - Local port forwarding

**Kill background tunnel:**
```bash
ps aux | grep 8888
kill [PID]
```

Or run without `-f` and use `Ctrl+C` to terminate.

### Remote Tunneling

**Use case**: Allow a remote server to access a service on your local machine.

**Syntax:**
```bash
ssh -R remote_port:local_host:local_port username@ssh_server
```

**Example**: Make your local web server (port 3000) accessible from remote server (port 8080):

```bash
ssh -f -N -R 8080:localhost:3000 username@remote_host
```

### Dynamic Tunneling (SOCKS Proxy)

**Use case**: Route all traffic through the SSH connection (acts as a VPN).

**Syntax:**
```bash
ssh -D local_port username@ssh_server
```

**Example**: Create SOCKS proxy on port 9090:

```bash
ssh -f -N -D 9090 username@remote_host
```

Configure your browser or application to use SOCKS proxy `127.0.0.1:9090`.

---

## SSH Escape Codes

SSH escape codes allow you to control active SSH connections.

### Accessing Escape Codes

Press `~` followed by a command character (only works after a newline).

### Common Escape Codes

| Code | Function |
|------|----------|
| `~.` | Disconnect (useful for frozen sessions) |
| `~^Z` | Place SSH session in background |
| `~#` | List forwarded connections |
| `~&` | Background SSH at logout when waiting for forwarded connections |
| `~?` | Display escape code help |
| `~C` | Open command line for modifying port forwarding |

### Example: Force Disconnect

If your SSH session is frozen:

1. Press `Enter`
2. Type `~.`

The connection will immediately close.

### Example: Modify Port Forwarding

1. Press `~C` to open SSH command line
2. Add forward: `-L 8080:localhost:80`
3. Press `Enter`

---

## Troubleshooting

### Permission Denied (publickey)

**Cause**: Server did not accept any authentication methods offered by the client.

**Common reasons:**
1. Public key not in `~/.ssh/authorized_keys`
2. Incorrect username
3. Wrong file permissions
4. Public key authentication disabled on server

**Solutions:**

**Check public key exists:**
```bash
ls ~/.ssh/authorized_keys
```

**Verify permissions:**
```bash
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

**Correct permissions:**
- `.ssh` directory: `700`
- `authorized_keys` file: `600`

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

**Ensure correct username:**
```bash
ssh user@remote_host
```

**Check server configuration:**
```bash
sudo grep PubkeyAuthentication /etc/ssh/sshd_config
```

Should be:
```
PubkeyAuthentication yes
```

### Connection Refused

**Cause**: Cannot reach the SSH daemon on the remote host.

**Common reasons:**
1. SSH daemon not running
2. Wrong port number
3. Firewall blocking connection

**Solutions:**

**Check SSH daemon status:**
```bash
sudo systemctl status sshd
```

**Start SSH daemon:**
```bash
sudo systemctl start sshd
```

**Check SSH port:**
```bash
sudo grep Port /etc/ssh/sshd_config
```

**Check firewall:**
```bash
sudo ufw status
sudo firewall-cmd --list-all
```

### Host Key Verification Failed

**Cause**: Server's host key has changed or doesn't match the stored key.

**Warning**: This could indicate a man-in-the-middle attack.

**Solution (if expected change):**

Remove old host key:
```bash
ssh-keygen -R remote_host
```

Or edit `~/.ssh/known_hosts` manually to remove the offending line.

### Too Many Authentication Failures

**Cause**: SSH client attempted too many authentication methods.

**Solution:**

Explicitly specify which key to use:
```bash
ssh -i ~/.ssh/specific_key username@remote_host
```

Or configure in `~/.ssh/config`:
```
Host remote_host
    IdentitiesOnly yes
    IdentityFile ~/.ssh/specific_key
```

### Using Verbose Output

For detailed debugging information:

```bash
ssh -v username@remote_host    # Verbose
ssh -vv username@remote_host   # More verbose
ssh -vvv username@remote_host  # Maximum verbosity
```

This shows:
- Connection attempts
- Authentication methods tried
- Configuration files read
- Key exchanges
- Cipher negotiations

---

## FAQs

### 1. What is SSH used for?

SSH is used to securely connect to and manage remote systems over a network. It allows users to:
- Log in to remote servers
- Run commands remotely
- Transfer files securely
- Forward network traffic through encrypted connections

SSH is commonly used for server administration, application deployment, remote troubleshooting, and secure file transfers using tools such as `scp` and `sftp`.

### 2. How does SSH differ from Telnet?

| Feature | SSH | Telnet |
|---------|-----|--------|
| **Encryption** | All data encrypted | Plaintext (no encryption) |
| **Security** | Secure | Insecure |
| **Authentication** | Keys or passwords | Passwords only |
| **Port** | 22 (default) | 23 (default) |
| **Modern use** | Standard for remote access | Obsolete for production |

### 3. Are SSH keys more secure than passwords?

**Yes.** SSH keys provide significantly stronger security:

- **Resistance to brute-force**: Key size makes guessing computationally infeasible
- **No credential reuse**: Each key can be unique per server
- **No transmission**: Private key never leaves local machine
- **Revocable**: Remove public key from server to revoke access
- **Passphrase protection**: Optional additional layer of security

### 4. Where are SSH keys stored?

**On the client (your computer):**
- Private key: `~/.ssh/id_rsa` (or `id_ed25519`, `id_ecdsa`)
- Public key: `~/.ssh/id_rsa.pub`
- Known hosts: `~/.ssh/known_hosts`
- Client config: `~/.ssh/config`

**On the server:**
- Authorized keys: `~/.ssh/authorized_keys`
- Server config: `/etc/ssh/sshd_config`
- Host keys: `/etc/ssh/ssh_host_*`

### 5. What is the authorized_keys file?

The `~/.ssh/authorized_keys` file contains a list of public keys (one per line) that are authorized to log in to a specific user account on the server.

When a client attempts to authenticate:
1. Server checks this file for matching public key
2. If found, server challenges the client
3. Client proves possession of private key
4. Access granted if verification succeeds

### 6. How do I restart the SSH service?

**Ubuntu/Debian:**
```bash
sudo systemctl restart ssh
```

**CentOS/RHEL/Fedora:**
```bash
sudo systemctl restart sshd
```

**Check status:**
```bash
sudo systemctl status ssh   # or sshd
```

### 7. Can I use SSH on Windows?

**Yes.** Multiple options:

1. **Windows 10/11** (built-in OpenSSH client):
   ```powershell
   ssh username@remote_host
   ```

2. **PuTTY**: Popular GUI SSH client for Windows

3. **Windows Subsystem for Linux (WSL)**: Full Linux environment

4. **Git Bash**: Includes SSH client

### 8. What port does SSH use?

**Default port: 22**

You can change it in `/etc/ssh/sshd_config`:
```
Port 2222
```

Connect to non-standard port:
```bash
ssh -p 2222 username@remote_host
```

### 9. Is it safe to change the SSH port?

**Changing the SSH port provides "security through obscurity":**

**Pros:**
- Reduces automated attacks on port 22
- Decreases log noise from scanners

**Cons:**
- Not a substitute for proper security measures
- Can cause connectivity issues with firewalls
- May complicate troubleshooting

**Recommendation**: Change the port as an additional measure, but prioritize:
- Disabling password authentication
- Using SSH keys
- Implementing fail2ban or similar tools
- Restricting user access

### 10. How do I disable password login in SSH?

Edit `/etc/ssh/sshd_config`:

```bash
sudo nano /etc/ssh/sshd_config
```

Set:
```
PasswordAuthentication no
```

**Before disabling:**
1. ✅ Ensure SSH key authentication works
2. ✅ Test login with SSH keys
3. ✅ Keep an active session open as backup

Restart SSH:
```bash
sudo systemctl restart ssh
```

---

## Conclusion

SSH is a core tool for securely accessing and managing remote systems. Using it effectively requires more than knowing how to connect—it also involves understanding authentication, trust relationships, configuration choices, and common failure scenarios.

This guide covered:
- ✅ SSH fundamentals and how it works
- ✅ Key-based authentication and trust mechanisms
- ✅ Generating and managing SSH keys
- ✅ Secure default configurations
- ✅ Advanced usage patterns (tunneling, agent forwarding)
- ✅ Practical troubleshooting techniques

Together, these topics provide a solid foundation for using SSH safely and confidently in everyday administrative and development tasks.

---

## Quick Reference Commands

### Key Generation
```bash
ssh-keygen                    # Generate default RSA key
ssh-keygen -t ed25519         # Generate Ed25519 key (recommended)
ssh-keygen -b 4096            # Generate 4096-bit RSA key
ssh-keygen -p                 # Change passphrase
ssh-keygen -l                 # Show fingerprint
```

### Connecting
```bash
ssh user@host                 # Basic connection
ssh -p 2222 user@host         # Custom port
ssh -i ~/.ssh/key user@host   # Specific key
ssh user@host command         # Execute single command
```

### Key Copying
```bash
ssh-copy-id user@host         # Copy public key to server
ssh-copy-id -i ~/.ssh/key.pub user@host  # Copy specific key
```

### Tunneling
```bash
ssh -L 8080:localhost:80 user@host       # Local tunnel
ssh -R 8080:localhost:80 user@host       # Remote tunnel
ssh -D 9090 user@host                    # Dynamic tunnel (SOCKS)
```

### Troubleshooting
```bash
ssh -v user@host              # Verbose output
ssh -vvv user@host            # Maximum verbosity
ssh-keygen -R host            # Remove host from known_hosts
```

### Configuration
```bash
sudo nano /etc/ssh/sshd_config    # Server config
nano ~/.ssh/config                # Client config
sudo systemctl restart ssh        # Restart SSH daemon
sudo sshd -t                      # Test config syntax
```

---

**Document Version**: 1.0  
**Last Updated**: 2026-02-10  
**Based on**: SSH Essentials: Working with SSH Servers, Clients, and Keys
