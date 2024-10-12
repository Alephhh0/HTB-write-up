# BoardLight

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
