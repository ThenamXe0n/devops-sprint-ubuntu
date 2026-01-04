# Set Up SSH Key-Based Authentication (Proper & Safe)
This step replaces password logins with cryptographic keys — the industry-standard way developers securely access servers.

But don’t worry — we will NOT disable password login yet.
We’ll only do that after you confirm your SSH key works.

---

> What Are SSH Keys (in Plain English)?
>SSH keys are a pair:

🔑 Private Key (kept on YOUR laptop)
- Never leaves your device
- Never shared
- Think of it like your house key

🏠 Public Key (stored on the server)
- Safe to share
- Used to verify your private key
- Think of it like the lock on your door

When you log in:

✔ Your server checks if your private key matches the saved public key
✔ No password travels over the network
✔ Hackers cannot brute-force it

This is why every serious engineering team uses SSH keys.
---
## steps to apply ssh key authentication
---

### -A — Generate SSH Keys on Your Local Machine
Do this on your local computer terminal (NOT the server).

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```
you'll see somthing like this 
```
Enter file in which to save the key:
```
> just press the enter by default it'll stored in /home/you/.ssh/id_ed25519
or on macOS) on /Users/you/.ssh/id_ed25519

then ask for passphase (press enter keep it empty)
🧠 Should you use a passphrase?

✔ Yes (recommended) — adds extra protection
❌ No, if you want convenience
| File             | Where       | Purpose                     |
| ---------------- | ----------- | --------------------------- |
| `id_ed25519`     | Your laptop | **Private key — KEEP SAFE** |
| `id_ed25519.pub` | Your laptop | **Public key — shareable**  |

---
### -B — Copy Your Public Key to the Server
login in as your non-root user 
```
ssh deploy@yourIp_address
```
then create the SSh folder (if not exists)
```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```
copy the whole line  - it start with : ssh ed25519
now on server open the authrized keys file :
```
nano ~/.ssh/authorized_keys
```
> Paste your key inside it.
> Save & exit:

CTRL + O → Enter

CTRL + X

set permissions : 
```
chmod 600 ~/.ssh/authorized_keys
```
> ## These permissions are critical — SSH will refuse weak ones.



