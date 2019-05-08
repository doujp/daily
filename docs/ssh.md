## Quick start: SSH key
To set up SSH key based authentication for your remote host:

Check to see if you already have an SSH key. The public key is typically located at ~/.ssh/id_rsa.pub on macOS / Linux, and at %USERPROFILE%\.ssh\id_rsa.pub on Windows.

If you do not have a key, run the following command in a terminal / command prompt to generate an SSH key pair:

```
ssh-keygen -t rsa -b 4096
```

Tip: Don't have ssh-keygen? Install a supported SSH client.

Add the contents of your local public key (the id_rsa.pub file) to the appropriate authorized_keys file(s) on the remote host.

On macOS / Linux, run the following command in a local terminal, replacing the user and host name as appropriate.

```
ssh-copy-id -p 22022 your-user-name-on-host@host-fqdn-or-ip-goes-here
```

On Windows, run the following commands in a local command prompt replacing the value of REMOTEHOST as appropriate.

```
SET REMOTEHOST=your-user-name-on-host@host-fqdn-or-ip-goes-here

scp -p 22022 %USERPROFILE%\.ssh\id_rsa.pub %REMOTEHOST%:~/tmp.pub
ssh -p 22022 %REMOTEHOST% "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat ~/tmp.pub >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && rm -f ~/tmp.pub"
```

## Improving your security with a dedicated key
While using a single SSH key across all your SSH hosts can be convenient, if anyone gains access to your private key, they will have access to all of your hosts as well. You can prevent this by creating a separate SSH key for your development hosts. Just follow these steps:

Generate a separate SSH key in a different file.

On macOS / Linux, run the following command in a local terminal:

```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa-remote-ssh
```

On Windows, run the following command in a local command prompt:

```
ssh-keygen -t rsa -b 4096 -f %USERPROFILE%\.ssh\id_rsa-remote-ssh
```

In VS Code, run Remote-SSH: Open Configuration File... in the Command Palette (F1), select an SSH config file, and add (or modify) a host entry as follows:

```
Host name-of-ssh-host-here
    User your-user-name-on-host
    HostName host-fqdn-or-ip-goes-here
    IdentityFile ~/.ssh/id_rsa-remote-ssh
```

Add the contents of the local id_rsa-remote-ssh.pub file generated in step 1 to the appropriate authorized_keys file(s) on the remote host.

On macOS / Linux, run the following command in a local terminal, replacing name-of-ssh-host-here with the host name in the SSH config file from step 2:

```
ssh-copy-id -p 22022 -i ~/.ssh/id_rsa-remote-ssh.pub name-of-ssh-host-here
```

On Windows, run the following commands in a local command prompt, replacing name-of-ssh-host-here with the host name in the SSH config file from step 2.

```
SET REMOTEHOST=name-of-ssh-host-here
SET PATHOFIDENTITYFILE=%USERPROFILE%\.ssh\id_rsa-remote-ssh.pub

scp -p 22022 %PATHOFIDENTITYFILE% %REMOTEHOST%:~/tmp.pub
ssh -p 22022 %REMOTEHOST% "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat ~/tmp.pub >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && rm -f ~/tmp.pub"
```