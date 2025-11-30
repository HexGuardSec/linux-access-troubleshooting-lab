# Scenario 1 — Invalid Shell in /etc/passwd

## 🎯 Goal

Simulate an SSH login issue caused by assigning an invalid shell (`/bin/false`) to a user.  
This demonstrates how an account can authenticate successfully but be disconnected immediately after login.

---

## 🧪 Steps

### 🔧 1. Create the user and set up SSH access

```bash
sudo adduser crashuser
sudo mkdir /home/crashuser/.ssh
sudo cp ~/.ssh/authorized_keys /home/crashuser/.ssh/
sudo chown -R crashuser:crashuser /home/crashuser/
sudo chmod 700 /home/crashuser/.ssh
sudo chmod 600 /home/crashuser/.ssh/authorized_keys
````

➡️ The user is now configured for SSH key-based login.

---

### 🛑 2. Simulate the broken shell

Change the user's shell to `/bin/false`, a special shell that exits immediately:

```bash
sudo usermod -s /bin/false crashuser
getent passwd crashuser
```

Expected output:

```
crashuser:x:1002:1002::/home/crashuser:/bin/false
```

---

### 🧪 3. Attempt SSH connection (from Kali)

```bash
ssh -p 2222 crashuser@192.168.56.106
```

Expected result:

```
Connection closed by remote host
```

➡️ SSH authenticates the user, but immediately terminates the session due to the invalid shell.

---

### 🛠️ 4. Fix the issue

Restore the default shell:

```bash
sudo usermod -s /bin/bash crashuser
getent passwd crashuser
```

Output:

```
crashuser:x:1002:1002::/home/crashuser:/bin/bash
```

---

### ✅ 5. Retest SSH login

```bash
ssh -p 2222 crashuser@192.168.56.106
```

Expected result:

```
Welcome to Ubuntu...
crashuser@ubuntu:~$
```

SSH login now works correctly.

---

## 📸 Screenshots

| Step | Filename                       |
| ---- | ------------------------------ |
| 1    | `01_create_crashuser.png`      |
| 2    | `02_usermod_shell_false.png`   |
| 3    | `03_ssh_connection_closed.png` |
| 4    | `04_fix_shell_bash.png`        |
| 5    | `05_ssh_success_after_fix.png` |

Place screenshots in the `screenshots/` folder and reference them in your GitHub repo or documentation site.

---

## 🧠 Key Takeaway

Setting a user's shell to `/bin/false` or `/usr/sbin/nologin` doesn't prevent authentication — it ends the session immediately after login. This is useful for disabling user interaction while keeping the account technically active.