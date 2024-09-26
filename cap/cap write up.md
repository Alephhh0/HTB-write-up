# Cap

Well, this is my first write up and I chose easy machine Cap from HTB.

## Enumeration

Just starting with Nmap:

```
nmap -sVC 10.10.10.245
```

![nmap](https://github.com/user-attachments/assets/428de170-61a9-4b2b-be77-85d76d020844)

Here we can see 3 tcp ports and on one of them there is FTP, in the future it may be useful. But now let’s check the website. Add this to hosts file (or the site will not work):

```
10.10.10.245 cap.htb
```

## Checking HTTP (port 80)

![site1](https://github.com/user-attachments/assets/2632adb9-433d-40ae-a186-bbc7a642a361)

Wow, it's logged as user. Here we can see different tabs: **IP config** shows output of the `ifconfig` command, **Network status** the output of the `netstat` but the "Security snapshot" tab caught my attention.

![image](https://github.com/user-attachments/assets/1ccaa9f8-46dd-47cf-a25e-2798dde41062)

when downloading, we get a **6.pcap** file, 
