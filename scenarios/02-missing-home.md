# Scenario 2 — Missing Home Directory

## 🎯 Goal

Simulate an SSH login where the user's home directory has been deleted.  
This helps demonstrate that authentication may still succeed, but the environment is broken or unstable.

---

## 🧪 Steps

### 🔧 1. Create user and set up SSH access

```bash
sudo adduser ghostuser
sudo mkdir /home/ghostuser/.ssh
sudo cp ~/.ssh/authorized_keys /home/ghostuser/.ssh/
sudo chown -R ghostuser:ghostuser /home/ghostuser/
sudo chmod 700 /home/ghostuser/.ssh
sudo chmod 600 /home/ghostuser/.ssh/authorized_keys
````

➡️ Then test login from Kali:

```bash
ssh -p 2222 ghostuser@192.168.56.106
```

✅ The login should succeed.

---

### 💣 2. Simulate the issue: delete the home directory

```bash
sudo rm -rf /home/ghostuser
```

➡️ Test SSH again:

```bash
ssh -p 2222 ghostuser@192.168.56.106
```

---

## 🧾 Expected Output

You may see:

```
Could not chdir to home directory /home/ghostuser: No such file or directory
Welcome to Ubuntu...
ghostuser@ubuntu:~$
```

Then run:

```bash
pwd
echo $HOME
```

Output:

```
/
home/ghostuser
```

The shell works, but the environment is degraded.

---

## 🛠️ Fix

Recreate the home directory:

```bash
sudo mkdir /home/ghostuser
sudo chown ghostuser:ghostuser /home/ghostuser
sudo chmod 755 /home/ghostuser
```

✅ Then test again:

```bash
ssh -p 2222 ghostuser@192.168.56.106
```

It should now behave normally.

---

## 📸 Screenshots

| Step                    | File                               |
| ----------------------- | ---------------------------------- |
| 1. User and SSH setup   | `01_create_ghostuser.png`          |
| 2. Deleted home dir     | `02_delete_home_dir.png`           |
| 3. SSH error message    | `03_ssh_no_home_warning.png`       |
| 4. Recreate home        | `04_recreate_home_dir.png`         |
| 5. Final successful SSH | `05_ssh_success_after_restore.png` |

---

## 🧠 Key Takeaway

SSH does not require a home directory to authenticate,
but `$HOME` missing can break tools, scripts, shell configs, and default behavior.