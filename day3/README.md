# Day 04: Disable Direct Root SSH Login

Part of the **KodeKloud 100 Days of DevOps** challenge.

| Item | Detail |
|------|--------|
| Challenge | 100 Days of DevOps (KodeKloud Engineer) |
| Day | 04 |
| Topic | Linux security, SSH hardening |
| Servers | All app servers: `stapp01`, `stapp02`, `stapp03` |
| Status | Completed |

---

## Task

After a security audit, disable direct SSH root login on all app servers in the
Stratos Datacenter.

## Real-world reason

`root` has full control of a server. If root can log in over SSH, an attacker
only has to guess one password to own the machine. Blocking direct root login
forces people to log in as a normal user and use `sudo`, which is safer and
leaves an audit trail of who did what.

## Approach

| Question | Answer |
|----------|--------|
| Which servers? | All 3 app servers |
| What to change? | Root SSH login |
| Which file? | `/etc/ssh/sshd_config` (server config, not `ssh_config`) |
| Which setting? | `PermitRootLogin no` |

## Commands (run on each app server)

```bash
ssh tony@stapp01     # then steve@stapp02, banner@stapp03
```

Set the value:

```bash
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
```

Check for overrides in drop-in config files:

```bash
sudo grep -ri PermitRootLogin /etc/ssh/sshd_config.d/
```

Validate the config and restart SSH:

```bash
sudo sshd -t
sudo systemctl restart sshd
```

## Verification

```bash
sudo grep -i '^PermitRootLogin' /etc/ssh/sshd_config
sudo systemctl status sshd
```

Expected output:

```
PermitRootLogin no
```

Optional test from the jump host (should be denied):

```bash
ssh root@stapp01
```

## Command breakdown

| Part | Meaning |
|------|---------|
| `sshd_config` | Config of the SSH **server** (`ssh_config` is the client) |
| `PermitRootLogin no` | Do not allow root to log in over SSH |
| `sed -i` | Edit the file in place |
| `sshd -t` | Test the config for mistakes before restarting |
| `systemctl restart sshd` | Apply the new setting |

## What I learned

- `sshd_config` (server) and `ssh_config` (client) are different files.
- A commented line (`#PermitRootLogin ...`) shows the default, it does not apply a rule.
- Drop-in files in `/etc/ssh/sshd_config.d/` can override the main file.
- Validate with `sshd -t` before restarting a remote SSH service.
- Security changes must be applied on every server, not just one.

---

[Back to main README](../README.md)
