---
tags:
  - linux
---
List swap configured
```bash
sudo swapon --show
```
List swap and memory ram enabled
```bash
free -h

               total        used        free      shared  buff/cache   available
Mem:           7.1Gi       6.0Gi       332Mi       195Mi       1.3Gi       1.1Gi
Swap:          1.1Gi       1.1Gi       684Ki

``` 
Create swap file
```bash
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
```
We need to mark the file as a swap file
```bash
sudo mkswap /swapfile
```
Now we can enabled the swapfile using the next command.
```bash
sudo swapon /swapfile
```
Now the swap is enabled but it's not permanent, if we want to do it permanent we need to do the next:
```bash
sudo cp /etc/fstab /etc/fstab.bak #backup this file
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
# References
If you want to check the [resource](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-22-04)
