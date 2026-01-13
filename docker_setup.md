# Docker CLI Permission Setup (Ubuntu)

This README explains how to **install Docker Engine on Ubuntu (using official Docker documentation)** and how to properly configure user permissions to avoid errors like:

```
permission denied while trying to connect to the Docker daemon socket
```

The guide also covers common issues with **different terminals (system terminal vs VS Code terminal)** and how to ensure Docker works everywhere.

---

## 1. Install Docker Engine (Official Method)

Docker should always be installed using the **official Docker documentation** to ensure correct packages, updates, and security.

➡️ Follow this official guide step by step:

**Docker Engine on Ubuntu**
https://docs.docker.com/engine/install/ubuntu/

After completing the installation, confirm Docker is installed by following the verification steps mentioned in the official documentation.

---

## 2. Post-Installation: Docker Without sudo

By default, Docker requires root privileges. Docker provides a **post-installation configuration** to allow non-root users to run Docker commands.

➡️ Follow the official post-install steps here:

https://docs.docker.com/engine/install/linux-postinstall/

This includes:
- Creating / using the `docker` group
- Adding the user to the `docker` group

---

## 3. Applying Group Changes (Very Important)

### Common misunderstanding
After adding a user to the `docker` group, Docker may still fail with permission errors even after logout/login.

### Why this happens
Group membership changes only apply to **new login sessions**. Many environments (GUI login, VS Code, SSH) reuse old sessions.

### Reliable solution
Start a **fresh login shell** for the user:

```
su - <username>
```

This guarantees the updated group permissions are loaded.

Verify:
```
groups
```

The output must include:
```
docker
```

At this point, Docker commands should work without `sudo`.

---

## 4. VS Code Terminal Permission Issue

### Problem
- Docker works in the system terminal
- Docker fails in VS Code terminal with a permission denied error

### Reason
VS Code terminals may run under an **older user session** that does not include updated group memberships.

---

### Recommended Fix

1. Close all VS Code windows
2. Open a system terminal
3. Start a fresh login shell:

```
su - <username>
```

4. Launch VS Code from that shell

```
code .
```

5. Inside the VS Code terminal, verify:

```
groups
```

Docker should now work correctly inside VS Code.

---

### Alternative (Per-Terminal Fix)

If VS Code is already open, start a fresh login shell directly inside the VS Code terminal:

```
su - <username>
```

---

## 5. Docker Socket Permissions (Verification)

Docker communicates through a Unix socket. Its permissions must allow access to the `docker` group.

Verify:

```
ls -l /var/run/docker.sock
```

Expected ownership:
- Owner: `root`
- Group: `docker`

If the socket ownership is incorrect, restarting Docker usually fixes it.

---

## 6. Final Verification

After completing all steps, the following commands should work **without sudo**:

```
docker ps -a
docker images
docker compose version
```

---

## Key Takeaway

> Docker permission issues are almost always caused by **session-related group changes**, not Docker itself.

Using a **fresh login shell** ensures Docker works consistently across system terminals, VS Code, and other development tools.

---

This setup follows **official Docker documentation** and applies to most Ubuntu-based systems.

