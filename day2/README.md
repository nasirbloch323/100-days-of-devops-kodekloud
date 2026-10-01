# Day 02: Temporary User Setup with Expiry

Part of the **KodeKloud 100 Days of DevOps** challenge.

| Item | Detail |
|------|--------|
| Challenge | 100 Days of DevOps (KodeKloud Engineer) |
| Day | 02 |
| Topic | Linux user management |
| Server | App Server 1 (`stapp01`), Stratos Datacenter |
| Status | Completed |

---

## Task

A developer named `ammar` needs temporary access to the `Nautilus` project.
Create a user `ammar` on App Server 1 with an account expiry date of
`2027-03-28`. The username must be lowercase.

## Real-world reason

Temporary developers and contractors should not keep access after their
project ends. An account left active is a security risk. Setting an expiry date
when the account is created means access is disabled automatically, with no one
needing to remember to remove it.

## Approach

| Question | Answer |
|----------|--------|
| Which server? | App Server 1 (`stapp01`) |
| Which user? | `ammar` (lowercase) |
| What condition? | Account expires on 2027-03-28 |
| Which flag? | `-e` (expire) |

## Commands

Log in to App Server 1:

```bash
ssh tony@stapp01
```

Create the user with an expiry date:

```bash
sudo useradd -e 2027-03-28 ammar
```

## Verification

Check the expiry date:

```bash
sudo chage -l ammar
```

Expected output includes:

```
Account expires : Mar 28, 2027
```

Confirm the user exists:

```bash
id ammar
```

## Command breakdown

| Part | Meaning |
|------|---------|
| `sudo` | Run with admin rights (only admins can create users) |
| `useradd` | Create a new user |
| `-e 2027-03-28` | Disable the account on this date (format `YYYY-MM-DD`) |
| `ammar` | Username |

## What I learned

- `useradd -e YYYY-MM-DD` sets an **account** expiry date.
- `chage -l <user>` shows account and password ageing details.
- Account expiry (`-e`) is different from password expiry (`-f`, `-M`).
- Usernames should follow the naming standard (lowercase).
- Temporary access should always have an end date.

## Quick reference

```bash
# Temporary user with expiry
sudo useradd -e YYYY-MM-DD username

# Change expiry of an existing user
sudo chage -E YYYY-MM-DD username

# Remove expiry
sudo chage -E -1 username
```

---

[Back to main README](../README.md)
