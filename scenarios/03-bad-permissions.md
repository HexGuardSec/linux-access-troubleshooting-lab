# Scenario 03 — SSH fails due to bad permissions on `.ssh` or `authorized_keys`

## 🛠️ Problem Description

A user is unable to authenticate via SSH, even though the correct public key is in place. The issue lies in insecure permissions on the `.ssh` directory or the `authorized_keys` file, which OpenSSH strictly enforces.

## 🧪 Simulation Steps

1. **Create the user:**

   ```bash
   sudo adduser crashuser
````

2. **Set up SSH key for the user:**

   ```bash
   sudo mkdir /home/crashuser/.ssh
   sudo cp /home/itadmin/.ssh/authorized_keys /home/crashuser/.ssh/
   sudo chown -R crashuser:crashuser /home/crashuser/.ssh
   sudo chmod 700 /home/crashuser/.ssh
   sudo chmod 600 /home/crashuser/.ssh/authorized_keys
   ```

3. **Break the permissions intentionally:**

   ```bash
   chmod 777 /home/crashuser/.ssh
   chmod 664 /home/crashuser/.ssh/authorized_keys
   ```

4. **Try SSH login:**

   ```bash
   ssh -p 2222 crashuser@192.168.56.106
   ```

   Result:

   ```
   Permission denied (publickey).
   ```

5. **Check logs on the server:**

   ```bash
   sudo tail -f /var/log/auth.log
   ```

   Example:

   ```
   Authentication refused: bad ownership or modes for directory /home/crashuser/.ssh
   ```

---

## 🧩 Root Cause

OpenSSH will refuse to use public key authentication if the permissions on `.ssh` or `authorized_keys` are too open. This is a default security behavior to prevent keys from being stolen or misused.

---

## ✅ Resolution

1. **Correct the permissions:**

   ```bash
   chmod 700 /home/crashuser/.ssh
   chmod 600 /home/crashuser/.ssh/authorized_keys
   chown -R crashuser:crashuser /home/crashuser/.ssh
   ```

2. **Retry SSH login:**

   ```bash
   ssh -p 2222 crashuser@192.168.56.106
   ```

   ➜ Connection should now succeed.

---

## ⚠️ NOTE for Ubuntu 24.04 & OpenSSH 9.x:

Setting `PasswordAuthentication no` is **not sufficient** to fully disable password fallback.
You **must also explicitly disable** `KbdInteractiveAuthentication` in your SSH configuration:

```conf
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
StrictModes yes
```

Otherwise, OpenSSH may silently fall back to password login using `keyboard-interactive`, even with `PasswordAuthentication no`.

> Always confirm with:
> `sshd -T | grep -i authentication`

---

## 📸 Screenshots (located in `screenshots/03-bad-permissions/`)

| Screenshot                                  | Description                                                |
| ------------------------------------------- | ---------------------------------------------------------- |
| `01_bad_permissions.png`                    | Shows insecure permissions on `.ssh` and `authorized_keys` |
| `02_auth_denied.png`                        | SSH access denied with `Permission denied (publickey)`     |
| `03_fix_permissions.png`                    | Commands used to fix permissions                           |
| `04_auth_success.png`                       | Successful SSH login after fix                             |
| `05_config_check.png`                       | SSHD config with `KbdInteractiveAuthentication no`         |
| `06_sshd_config_effective.png` *(optional)* | Output of `sshd -T` confirming active settings             |

---

## 🧠 Key Takeaway

Even a correct key setup can fail if file permissions are too open.
Always verify permissions, ownership, and SSH configuration settings.

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chown -R user:user ~/.ssh
