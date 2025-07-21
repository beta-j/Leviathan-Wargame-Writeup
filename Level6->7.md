**Username:** ``leviathan5``

**Password:** ``szo7HDB88w``


#


We start off the challenge by logging in as ``Leviathan5`` over ssh on Port ``2223``:

```console
ssh leviathan6@leviathan.labs.overthewire.org -p 2223
```

Just like the previous level, we have an executable in our home directory called `leviathan6`.  When executed we are told that it is expecting a four-digit code.
```console
leviathan6@gibson:~$ ls -la
total 36
drwxr-xr-x  2 root       root        4096 Apr 10 14:23 .
drwxr-xr-x 83 root       root        4096 Apr 10 14:24 ..
-rw-r--r--  1 root       root         220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root       root        3771 Mar 31  2024 .bashrc
-r-sr-x---  1 leviathan7 leviathan6 15036 Apr 10 14:23 leviathan6
-rw-r--r--  1 root       root         807 Mar 31  2024 .profile
leviathan6@gibson:~$ ./leviathan6
usage: ./leviathan6 <4 digit code>
```

A four digit code means a total of 10,000 total possible combinations (from `0000` to `9999`), which would perhaps take a few hours to try one-by-one.  But if we can put together a simple script, we should be able to try all the combinations in just a few seconds.


