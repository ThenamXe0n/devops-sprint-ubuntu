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
---
### -E — Add the Docker Repository
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
now your server knows where to get official Docker build 
---

### -F — Update again
```
sudo spt update
```
### -G — Install Docker Engine + CLI + Containerd + Compose Plugin
```
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---
This installs:
- docker-ce → The Docker engine
- docker-cli → Command tool
- containerd → Runtime
- buildx → Advanced builds
- compose-plugin → Modern Docker Compose

> 🧠 Note: The old docker-compose Python version is deprecated.
Professionals now use the Compose plugin → docker compose (space, not dash).

---
### -H — Verify Docker Works
```
sudo docker run hello-world
```
Expected output:
Docker pulls a tiny test container and prints a success message 🎉

> note : This is security-critical.

---
### -I — Allow your user to run Docker WITHOUT sudo
```
sudo usermod -aG docker $USER
```
now log out and log back in: 
```
exit
```

---
### -J — Verify Docker Compose
```
Docker Compose version V2.x.x
```
This confirms the plugin version is installed — modern & supported
---

### How Industry Engineers Install Docker
You just followed the official and recommended method.
Alternatives used in real life:

---
### 🛡 Security Notes (Important But Simple)

Docker group members can control the Docker daemon — meaning they effectively have root-like power.

This is normal & expected in DevOps.

Companies control access using:

- ✔ IAM
- ✔ SSH keys
- ✔ VPN / Bastion
- ✔ Audit logs

So don’t casually add random users to the docker group.
---

### 🎯 At the End of Step 4 — You Will Have:

✔ Docker installed
✔ Docker Compose installed
✔ Non-root Docker usage
✔ Test containers running
✔ Clean, secure setup

This is the foundation of your deployment pipeline.

> Next steps after this will be:

➡️ Step 5 — Install Node (for local builds & debugging only)
➡️ Step 6 — Setup NGINX reverse proxy (production-grade)
➡️ Step 7 — Structure your MERN + Next.js apps for Docker
➡️ Step 8 — Docker Compose for multi-service apps
➡️ Step 9 — CI/CD (GitHub Actions → server deploy)

All step-by-step with deep explanations — no rushing.

---
# Install Node.js (for optional local debugging & utility use — NOT for production runtime, since Docker handles that)

### -A — Add NodeSource Repo
```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
```
This script:

- ✔ Securely adds the NodeSource signing key
- ✔ Adds the apt repository
- ✔ Prepares your system for Node.js 20 LTS

> 🧠 Why Node 20?
  Because it is LTS — Long Term Support, which is what production environments use.

### -B — Install Node.js
```
sudo apt install -y nodejs
```
This installs:

✔ node — the runtime
✔ npm — package manager 



