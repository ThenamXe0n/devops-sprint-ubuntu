# Fresh VPS Initial Security & System Prep
This step is about turning your raw VPS into a safe and professional Linux server foundation.
Everything else (Docker, NGINX, CI/CD, apps) sits on top of this — so we do it right first.

We'll do : 
1. log in as root
2. create a non-root user
3. Give them admin (sudo) rights
4. Secure SSH
5. Enable a firewall (UFW)
6. Update the system


> ### Why do we NOT use the root account daily?.
> Root = God mode on the server. No restrictions.
One mistake → delete the system, expose security, or let attackers abuse your machine.

 Industry Standard:
- [X] DevOps engineers never develop or deploy as root
- [X] Root is only used for emergency admin
- [X] Everyday work runs as a normal user with sudo

 ## Log into your VPS as root
 from your local machine terminal : 
 ```bash
ssh root@your_server_ip
```
>If your provider gave you a password, enter it.
>If using SSH keys — you’ll be logged in automatically.

## Create a new user
Pick a username like deploy, admin, or your name.
```bash
adduser deploy
```
> It will ask:
- password
- full name (optional)
- other details (you can press Enter to skip)

> ### What this really does?
> It creates:
>  - a home directory → /home/deploy
>  - a shell environment
>  - local settings

### Give the user admin (sudo) rights

```bash
usermod -aG sudo deploy
```
Now this user can run admin commands like:
```bash
sudo apt update
```
but must confirm with password — this prevents accidental disasters.

> ## Industry Notes
> Many companies use sudoers group for controlled privilege escalation.
Some restrict who can run what.

Example advanced method (not required now):
```
visudo
```
- Then define exact allowed commands.


### Log in as the new user (test it)
```
su -deploy
```
switch user from root to deploy

now try 
```
sudo ls /root
```
> If it asks for password → ✔ working.


### Secure SSH (VERY IMPORTANT)
Right now your server allows:

❌ direct root login
❌ password login (hackable)

We’ll change that.
Open SSH config:
```
sudo nano /etc/ssh/sshd_config
```
Find and update lines (uncomment if needed):
```
PermitRootLogin no
PasswordAuthentication no
```

> What these do:

> ✔ Blocks root logins
> ✔ Blocks password logins → SSH key required (best security)

💡 Alternative — allow password logins temporarily
If you don't yet use SSH keys, set:
```
PasswordAuthentication yes
```

We can secure later.
- Save + exit Nano:
- CTRL + O → Enter
- CTRL + X

```
sudo systemctl restart ssh
```
### Enable & Configure Firewall (UFW)
UFW = Uncomplicated Firewall
It locks down network ports.

Allow SSH first (or you'll get locked out):
```
sudo ufw allow OpenSSH
```
then enable firewall
```
sudo ufw enabe
```
check status 
```
sudo ufw status
```
expected output will show something like :
```
OpenSHH ALLOW 
```
Later we will open:

✔ HTTP (80)
✔ HTTPS (443)
✔ Custom app ports if needed


### Update & Upgrade System Packages
---
```bash
sudo apt update && sudo apt upgrade -y
```
This ensures:

✔ latest security patches
✔ latest kernel + libs
✔ stable server before Docker setup

Industry-grade best practice.
---
### At the end of Step 1 — You Now Have
---
✔ A secure admin user
✔ Root login disabled
✔ Password login disabled (if you chose keys)
✔ Firewall enabled
✔ System updated

This is exactly how professional DevOps teams prepare production servers.

🧑‍💻 Industry Alternatives & Notes
🔐 Authentication methods

Engineers use:

Method	Security	Notes
Password login	❌ Weak	Brute-force risk
SSH keys	✅ Strong	Best practice
SSH + 2FA	🔒 Enterprise	Adds Google Authenticator
🔥 Firewall alternatives

Some companies use:

iptables (advanced raw firewall)

cloud firewalls (AWS Security Groups / DO Networking / GCP)

But UFW is perfect & standard for VPS.

🏗 User & privilege management styles
Approach	Used by
1 admin user + sudo	Most companies
Separate deploy user	Startups / DevOps
IAM / SSO integration	Enterprises

We’ll use admin + deploy user model, clean & scalable.





