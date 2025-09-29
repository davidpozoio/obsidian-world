---
tags:
  - linux
---

to add a new user to the sudoers file you need to execute the next commands:
```bash
su - # log as root
nano /etc/sudoers # open sudoers file
```
When you open the sudoers file you need to add the next line:
```txt
username ALL=(ALL:ALL) ALL
```
