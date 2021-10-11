## Activity 11: Cracking Hashes

You should have a web browser open and also open the AttackBox on TryHackMe to complete the activity.


### Task 2

You can use online tools such as [crackstation](https://crackstation.net/) and [hash-analyzer](https://www.tunnelsup.com/hash-analyzer/) or installed 
tools such as `hash-identifier` and `hashcat` to analyze and crack hashes.

#### First hash

To crack the first hash `48bb6e862e54f2a795ffc4e541caed4d`, you can use online tool such as [crackstation](https://crackstation.net/) or you can use `hash-identifier`
to figure out that it is MD5 hash function. The answer is `easy`.

```
root@kali:~# hash-identifier
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 48bb6e862e54f2a795ffc4e541caed4d

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
```

Then use `hashcat` to crack it:

```
hashcat -m 0 "48bb6e862e54f2a795ffc4e541caed4d" Tools/wordlists/rockyou.txt
```

#### Second hash

To crack the second hash `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`, you can use similar strategy. It uses `Sha1` function. The answer is `password123`

#### Third hash

Third hash uses `Sha256`, which is mode `1400` in hashcat. 

```
hashcat -m 1400 "1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032" Tools/wordlists/rockyou.txt
```

Use --show to see the cracked hash:

```
hashcat -m 1400 --show "1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032" Tools/wordlists/rockyou.txt
```

### Task 4

Use john tool for these cracks. You can use `hash-identifier` to identify the type of the hash function first.

```
john --format=raw-md5 --wordlist=Tools/wordlists/rockyou.txt task4_hashes/hash1.txt
```

Then to see the cracked hash:

```
john task4_hashes/hash1.txt --show --format=raw-md5
```

You can see the list of formats by:

```
john --list=formats | grep -iF "sha1"
```

**Answers:**

1. md5
2. biscuit
3. sha1
4. kangeroo
5. sha256
6. microphone
7. whirlpool
8. colossal


### Task 5

Answer: `mango`

```
python3 /opt/john/ssh2john.py idrsa.id_rsa > id_rsa_hash.txt
```

```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

