### Names: Claire, Megan, Danny, Kai'lani, Wes, Chris, Caden


#### Definitions

**Salting a password:** Salting a password is when you add a pre-defined phrase like "new" to a password before it gets hashed as an additional layer of security

**Escaping text editors:** Many linux text editors allow you to enter linux commands from within the editor, which can give you a root shell if the editor was started with sudo


**LinEnum:** simple bash script that performs common commands related to privilege escalation. https://github.com/rebootuser/LinEnum/blob/master/LinEnum.sh

#### Commands

```chmod +x FILENAME.sh```
Creates an executable SH file.

```find / -perm -u=s -type f 2>/dev/null```
Used to search manually for SUID/GUID files on the system. (Similar to LinEnum)
