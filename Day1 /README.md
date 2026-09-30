# Day 01: Create User with Non-Interactive Shell

## Task
Create user `mark` with a non-interactive shell on App Server 1.

## Real-world reason
Backup agents need a service account. A nologin shell stops humans
from logging in with it, so a leaked password is not a server breach.

## Commands
ssh tony@stapp01
sudo useradd -s /sbin/nologin mark

## Verify
grep mark /etc/passwd

## What I learned
- `-s` sets the shell
- /sbin/nologin blocks interactive login
