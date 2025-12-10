# Linux Access Troubleshooting Lab

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Category](https://img.shields.io/badge/Category-Troubleshooting-blue)
![SSH](https://img.shields.io/badge/SSH-Access%20Issues-red)
![Permissions](https://img.shields.io/badge/Permissions-Root%20Cause%20Analysis-lightgrey)
![Logs](https://img.shields.io/badge/Logs-Audit%20%7C%20journalctl-blueviolet)

**Hands-on scenarios for diagnosing and fixing broken SSH access on Linux systems.**  
This project simulates real-world issues encountered by IT administrators and support engineers when dealing with user authentication problems.

---

## 🧪 Scenarios Covered

| # | Scenario Title | Description |
|---|----------------|-------------|
| 1 | Invalid shell in `/etc/passwd` | SSH connects, then closes instantly |
| 2 | Missing home directory | SSH works with warning, limited access |
| 3 | Incorrect permissions on `.ssh/` | SSH key is refused |
| 4 | Deleted `authorized_keys` | User can't authenticate |
| 5 | Denied in `sshd_config` | Access explicitly blocked |
| 6 | Locked user account | Login fails despite valid key |
| 7 | Bad group or UID | SSH connection unstable |

---

## 📂 Folder Structure

```

linux-access-troubleshooting-lab/
├── README.md
├── scenarios/
│   ├── 01-invalid-shell.md
│   ├── 02-missing-home.md
│   ├── 03-permissions-error.md
│   └── ...
├── screenshots/
│   ├── 01-invalid_shell/
│   ├── 02-permission_denied/
│   ├── 03-fixed_access/

```

---

## 🧠 Skills Practiced

- User management (`usermod`, `useradd`, `passwd`, `chage`)
- File permissions (`chmod`, `chown`, `ls -l`, `umask`)
- SSH troubleshooting (`auth.log`, `journalctl`, `sshd_config`)
- Security practices (`AllowUsers`, `DenyUsers`, shell locking)

---

## 🔐 Environment

- Ubuntu Server 22.04 LTS
- Kali Linux Client
- OpenSSH server on port 2222
