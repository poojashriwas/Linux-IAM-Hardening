# Linux-IAM-Hardening
"Mini project on Linux IAM and System Hardening"

#  Linux IAM & System Hardening — Secure User, Group & Permissions

A hands-on cybersecurity project focused on **Identity & Access Management (IAM)**, **Linux Hardening**, and **least-privileged access controls** on Ubuntu.

This project demonstrates secure configuration of **users, groups, permissions, sudo policies, ACLs, and audit logs**, along with **misconfiguration detection & remediation**.

---

##  Project Objectives

* Build a secure IAM model on Ubuntu (Users & Groups)
* Enforce **least-privilege sudo access**
* Apply **POSIX permissions & Access Control Lists**
* Enable **auditd** to monitor critical files
* Identify & fix **realistic Linux misconfigurations**
* Validate with attacker simulation (Kali VM)

---

##  Tools & Environment

| Component           | Description                                         |
| ------------------- | --------------------------------------------------- |
| Ubuntu 22.04 VM     | Target Secure System                                |
| Kali Linux          | Attacker / Testing VM                               |
| auditd              | System auditing & monitoring                        |
| VirtualBox / VMware | Virtualization platform                             |
| Bash Commands       | `useradd`, `groupadd`, `chmod`, `visudo`, ACL tools |

---

##  IAM Design — Roles & Access Policy

| Role              | Group          | Access                                                    |
| ----------------- | -------------- | --------------------------------------------------------- |
| **Admin**         | `admin`        | Limited sudo — user mgmt, services, packages              |
| **Developer**     | `dev`          | Restart application service only, write to project folder |
| **Auditor**       | `auditor`      | Read-only access, no sudo                                 |
| **Collaborators** | `project-team` | Shared folder write access                                |

✅ **NO NOPASSWD sudo**
✅ **Home directories = private**
✅ **Root SSH login disabled**

---

##  Sudo Policy (Least Privilege)

Allowed commands via `/etc/sudoers`:

* `/usr/sbin/useradd, usermod, userdel`
* `/usr/bin/apt-get`
* `systemctl restart myapp.service` (dev only)

---

##  File & Directory Security

| Path           | Permission | Reason                       |
| -------------- | ---------- | ---------------------------- |
| `/srv/project` | `2775`     | Group inheritance via setgid |
| `/etc/passwd`  | `644`      | Read-only system file        |
| `/etc/shadow`  | `640`      | Protect password hashes      |
| `/home/*`      | `700`      | Private home folders         |

---

##  Auditing (auditd Enabled)

Monitoring rules added:

```bash
-w /etc/sudoers -p wa -k sudoers_changes
-w /etc/passwd -p wa -k passwd_changes
```

Review logs:

```bash
ausearch -k sudoers_changes
aureport -f
```

---

##  Remediation of Misconfigurations

| Misconfiguration           | Risk                 | Fix                                |
| -------------------------- | -------------------- | ---------------------------------- |
| World-writable cron file   | Privilege escalation | `chmod o-w /etc/cron.d/<file>`     |
| `NOPASSWD: ALL` in sudoers | Full root compromise | Remove NOPASSWD, restrict commands |
| Readable `/etc/shadow`     | Password hash theft  | `chmod 640 /etc/shadow`            |

---

##  Testing & Verification (Kali VM Attack Simulation)

✔️ SSH access tested
✔️ Unauthorized sudo attempts denied
✔️ Developer only allowed to restart specific service
✔️ Invalid privilege escalation blocked

---

## ✅ Security Best Practices Implemented

* 🔒 Strong password policy
* 🛂 Least-privilege sudo rules
* 📂 Secure file permissions & ACLs
* 🔍 auditd change tracking
* ⚠️ No passwordless sudo
* 👤 Remove inactive/test users regularly
* ⏳ Password expiration policy

---

##  Commands Used (Quick View)

Create Groups & Users

```bash
groupadd admin
groupadd dev
groupadd auditor
groupadd project-team
useradd bob -g dev -m
```

Set Permissions & ACL

```bash
chmod 2775 /srv/project
setfacl -m g:project-team:rwx /srv/project
```

Enable auditd

```bash
sudo apt install auditd -y
sudo systemctl enable --now auditd
```

---

##  Demonstration Screens

> ✅ IAM Setup
> ✅ Sudo enforcement
> ✅ ACL implemented
> ✅ auditd logs
> ✅ Vulnerability fixes
> ✅ Attacker test output

*(Add your screenshots here)*

---

##  Learning Outcomes

* Secure Linux IAM architecture
* Real-world misconfiguration analysis
* Hardening & auditing practices
* Practical cybersecurity operations
* Ethical hacking mindset

---

##  Project Highlights

* Enterprise-style access model
* Real-world Linux hardening tasks
* Offensive + Defensive security
* Audit & compliance focused

---

## 📍 Conclusion

This project strengthens fundamental cybersecurity and Linux sysadmin skills by applying **Zero-Trust principles**, **privileged access controls**, and **system-level auditing**.

Perfect for SOC, Cybersecurity Analyst, DevSecOps, and Red-Blue team learning.
