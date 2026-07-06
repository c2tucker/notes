# Linux Command Cheat Sheet
**Curtis Tucker | Home Lab & Pre-Job Sprint | June 2026**

---

## systemctl — Service Management

| Command | What it does |
|---|---|
| `systemctl status X` | Check current state of service |
| `systemctl start X` | Start service now |
| `systemctl stop X` | Stop service now |
| `systemctl restart X` | Stop and start again |
| `systemctl enable X` | Auto-start on boot |
| `systemctl disable X` | Don't auto-start on boot |
| `systemctl enable --now X` | Enable AND start immediately |
| `systemctl list-units --type=service --state=running` | List all running services |
| `systemctl list-units --type=service --state=failed` | List all failed services |

**Key concept:** `start/stop` = right now. `enable/disable` = survives reboot.

---

## ip — Network Interfaces & Routing

| Command | What it does |
|---|---|
| `ip a` | Show all interfaces and IPs |
| `ip addr` | Same as above (long form) |
| `ip addr show eth0` | Show specific interface |
| `ip route` | Show routing table |
| `ip link` | Show interface state only |

**Key concept:** `lo` = loopback (127.0.0.1, talks to itself). `eth0` = real network interface.
**Default gateway** = the exit door all internet traffic goes through.

---

## ss — Socket Statistics (Open Ports & Connections)

| Command | What it does |
|---|---|
| `ss -tlnp` | Show listening TCP ports |
| `ss -tulnp` | Show listening TCP and UDP ports |
| `ss -tnp` | Show active TCP connections |
| `ss -a` | Show everything |

**Key concept:** `LISTEN` = port is open waiting for connections. Common ports: 22=SSH, 80=HTTP, 443=HTTPS.

---

## curl — Transfer Data via URL

| Command | What it does |
|---|---|
| `curl URL` | Basic request, shows response body |
| `curl -I URL` | Headers only |
| `curl -v URL` | Verbose, shows full conversation |
| `curl -L URL` | Follow redirects |
| `curl -o file URL` | Save response to a file |
| `curl -s URL` | Silent, no progress output |
| `curl -o /dev/null -s -w "%{http_code}\n" URL` | Show HTTP status code only |

**Common HTTP status codes:**
- 200 = OK
- 301 = Permanently moved
- 302 = Temporarily moved
- 401 = Unauthorized
- 403 = Forbidden
- 404 = Not found
- 500 = Server error
- 000 = Can't connect at all

---

## dig — DNS Lookups

| Command | What it does |
|---|---|
| `dig domain` | Full A record lookup |
| `dig domain +short` | Just the IP, clean output |
| `dig domain MX` | Mail server records |
| `dig domain NS` | Nameserver records |
| `dig domain AAAA` | IPv6 address |
| `dig domain TXT` | Text records |
| `dig @8.8.8.8 domain` | Query specific DNS server |
| `dig @8.8.8.8 domain +short` | Clean output via specific DNS server |

**DNS record types:**
- `A` = IPv4 address
- `AAAA` = IPv6 address
- `MX` = Mail server
- `CNAME` = Alias to another domain
- `TXT` = Text/verification records
- `NS` = Nameservers for the domain

**TTL** = Time To Live — how long the answer is cached before asking again.

---

## ssh — Secure Shell

| Command | What it does |
|---|---|
| `ssh user@host` | Connect to remote machine |
| `ssh-keygen -t ed25519` | Generate a new key pair |
| `ssh-copy-id user@host` | Copy public key to remote machine |
| `ssh-add ~/.ssh/id_ed25519` | Load key into agent |
| `ssh-add -l` | List keys loaded in agent |
| `ssh-keygen -l -f key.pub` | Show key fingerprint |
| `cat ~/.ssh/config` | View SSH shortcuts/aliases |

**How key-based auth works:**
- Private key (`id_ed25519`) = stays on YOUR machine, never share
- Public key (`id_ed25519.pub`) = placed on remote machines you want to access
- SSH agent = holds your key in memory so you don't retype passphrase repeatedly

**SSH config file (`~/.ssh/config`) shortcut format:**
```
Host ubuntu
    HostName 10.0.0.143
    User server
```
Lets you type `ssh ubuntu` instead of `ssh server@10.0.0.143`

---

## Bash Scripts Built

| Script | What it does |
|---|---|
| `daily_system_check_v1.sh` | System health report — hostname, disk, memory, users, disk warning |
| `backup.sh` | Takes a target directory, creates timestamped tarball, saves to ~/backups |
| `create_user.sh` | Creates a new user with home directory and sudo access |
| `remove_user.sh` | Safely removes a user with guard clauses and confirmation prompt |
| `service_checker.sh` | Checks status of critical services using a loop |

**GitHub repo:** github.com/c2tucker/bash-scripts

---

## Cron Jobs Configured

```
0 8 * * * /home/curtistucker/Scripts/daily_system_check_v1.sh >> /home/curtistucker/logs/daily_check.log 2>&1
0 8 * * * /home/curtistucker/Scripts/service_checker.sh >> /home/curtistucker/logs/service_check.log 2>&1
```

**Cron time format:** `minute hour day month weekday command`
- `0 8 * * *` = 8:00am every day
- `*` = every (any value)
- `>>` = append to log file
- `2>&1` = capture errors to same log file

---

## Git Workflow

```bash
git status              # see what changed
git add filename        # stage a file
git add .               # stage everything
git commit -m "msg"     # snapshot with message
git push                # send to GitHub
```

**One-time setup:**
```bash
git config --global user.name "Your Name"
git config --global user.email "you@email.com"
git config --global init.defaultBranch main
git config --global credential.helper store
```

---

## Lab Infrastructure

| Machine | Access | IP | User |
|---|---|---|---|
| ubuntu-lab | `ssh ubuntu-lab@orb` | OrbStack | curtistucker |
| rocky-lab | `ssh rocky-lab@orb` | OrbStack | curtistucker |
| node-1/2/3 | `ssh node-1@orb` | OrbStack | curtistucker |
| ubuntuserver1 | `ssh ubuntu` | 10.0.0.143 | server |
| Surface/Mint | `ssh mint` | 10.0.0.203 | c2tucker |

---

*Last updated: June 24, 2026*
