**Username:** ``leviathan5``

**Password:** ``0dyxT7F4QD``


#


We start off the challenge by logging in as ``Leviathan5`` over ssh on Port ``2223``:

```console
ssh leviathan5@leviathan.labs.overthewire.org -p 2223
```

Looking at the contents of our home directory, this time we are presented with an executable called ``leviathan5`` which has the SUID bit set to run as user leviathan6.  Running the executble simply returns a message saying `Cannot find /tmp/file.log`
```console
leviathan5@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Apr 10 14:23 .
drwxr-xr-x 83 root       root        4096 Apr 10 14:24 ..
-rw-r--r--  1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31  2024 .bashrc
-r-sr-x---  1 leviathan6 leviathan5 15144 Apr 10 14:23 leviathan5
-rw-r--r--  1 root       root         807 Mar 31  2024 .profile
leviathan5@gibson:~$ ./leviathan5
Cannot find /tmp/file.log
```
