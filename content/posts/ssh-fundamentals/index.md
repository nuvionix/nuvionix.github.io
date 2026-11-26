---
title: "SSH Fundamentals: Secure Remote Access"
date: 2026-11-26
draft: false
description: "Learn how SSH works, understand authentication methods, and explore common SSH commands and configuration."
tags: ["SSH", "Linux", "Networking", "Cybersecurity", "Remote Access"]
categories: ["Networking"]
---

## Introduction

SSH stands for Secure Shell. It is a network protocol used to establish secure connections between computers over an unsecured network.

SSH is commonly used by system administrators and developers to access remote servers, execute commands, transfer files, and manage infrastructure.

Unlike older remote-access methods that may transmit information without proper protection, SSH encrypts communication between the client and server.

## How SSH Works

SSH uses a client-server model.

1. The SSH client runs on the user's computer.
2. The client connects to an SSH server.
3. The server presents its host identity.
4. The client and server establish encrypted communication.
5. The user authenticates.
6. A secure remote session becomes available.

SSH commonly uses TCP port 22 by default, although administrators can configure another port.

## Basic SSH Command

The general command format is:

```bash
ssh username@server-address

For example:
ssh user@example.com


This attempts to connect to the remote server using the specified username.
Authentication Methods
Password Authentication
The user provides a password to authenticate with the remote account.
Public-Key Authentication
Public-key authentication uses a key pair:
- Private Key: Kept securely by the user.
- Public Key: Added to the remote account's authorized keys.
The private key should never be shared with other people.
Generating an SSH Key Pair
A common command for creating an Ed25519 key pair is:
ssh-keygen -t ed25519 -C "your-email@example.com"


The command generates a private key and a corresponding public key.

The public key can be displayed with:
cat ~/.ssh/id_ed25519.pub


Copying Files with SCP
SCP can transfer files through an SSH connection.

To copy a local file to a remote server:
scp file.txt username@server-address:/remote/path/


To copy a remote file to the local computer:
scp username@server-address:/remote/path/file.txt .


SSH Configuration
Users can create an SSH configuration file at:
~/.ssh/config


Example configuration:
Host myserver
    HostName example.com
    User username
    IdentityFile ~/.ssh/id_ed25519


After configuring the host, the user can connect with:
ssh myserver


Security Best Practices
- Keep private keys confidential.
- Use strong authentication methods.
- Verify server host keys.
- Protect private keys with appropriate permissions.
- Keep SSH software updated.
- Disable unnecessary accounts and access methods.
- Apply firewall rules and access restrictions.
Conclusion
SSH is an essential tool for secure remote administration and file transfer. Understanding its connection process, authentication methods, and configuration helps developers and administrators manage remote systems more safely.
