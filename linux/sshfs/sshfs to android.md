You need to instal f-droid, this is an application to install primitive ftp, this application give us sftp and ftp connection we setup the user and password and port, and finally we need to install sshfs in our computer and execute this command.

```bash
sshfs USERNAME@ANDROID_IP:/sdcard ./MOUNT_FOLDER -p 2222
```
And now we can have access to our android files from our computer.
We can turn off our ssh android server anyway to prevent security problems.