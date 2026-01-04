# Install Docker & Docker Compose the Right Way (on Ubuntu)
From here on, everything we deploy (Node, MongoDB, Nginx, Next.js, CI/CD) will run inside Docker containers.

So this step is about:

✔ Installing Docker
✔ Installing Docker Compose (plugin method — industry-preferred)
✔ Making Docker usable without sudo
✔ Verifying everything works

And of course — I’ll explain the WHY behind each part.
---
> 🧠 Why Docker? (Short but deep)

Without Docker:
- Each app needs its own Node version
- Dependencies pollute the server
- “Works on my machine” becomes a nightmare
- Deploying updates risks breaking existing apps

With Docker:

- Each app runs in its own isolated box (container)
- Same environment everywhere — laptop → staging → production
- Rolling updates become easy
- Reproducibility & portability increase dramatically
This is exactly how modern companies run apps.
---

> # -A — Uninstall any old Docker versions (safeguard)
```
sudo apt remove docker docker-engine docker.io containerd runc
```
### B — Update your package list
```
sudo apt update
```
This ensures your system knows about the latest package versions.

### -C — Install required dependencies
These packages allow apt to use HTTPS repos securely:
```
sudo apt install -y ca-certificates curl gnupg
```
### -D — Add Docker’s official GPG key
This proves packages really come from Docker and not a fake source.
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
> note : This is security-critical.
---
# Install Node.js (for optional local debugging & utility use — NOT for production runtime, since Docker handles that)


