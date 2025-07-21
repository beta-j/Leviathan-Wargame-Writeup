**Username:** ``leviathan4``

**Password:** ``WG1egElCvO``


#


We start off the challenge by logging in as ``Leviathan4`` over ssh on Port ``2223``:

```console
ssh leviathan4@leviathan.labs.overthewire.org -p 2223
```

Looking at the contents of our home directory, this time we find a hidden folder called `.trash` that belongs to the `leviathan4` user group:

```console
leviathan4@gibson:~$ ls -la
total 24
drwxr-xr-x  3 root root       4096 Apr 10 14:23 .
drwxr-xr-x 83 root root       4096 Apr 10 14:24 ..
-rw-r--r--  1 root root        220 Mar 31  2024 .bash_logout
-rw-r--r--  1 root root       3771 Mar 31  2024 .bashrc
-rw-r--r--  1 root root        807 Mar 31  2024 .profile
dr-xr-x---  2 root leviathan4 4096 Apr 10 14:23 .trash
```

We can also see that the `.trash` folder contains a single executable file called `bin`:
```console
leviathan4@gibson:~$ ls -la .trash/
total 24
dr-xr-x--- 2 root       leviathan4  4096 Apr 10 14:23 .
drwxr-xr-x 3 root       root        4096 Apr 10 14:23 ..
-r-sr-x--- 1 leviathan5 leviathan4 14940 Apr 10 14:23 bin
leviathan4@gibson:~$ ls -la .trash/bin
-r-sr-x--- 1 leviathan5 leviathan4 14940 Apr 10 14:23 .trash/bin
```

Running the executable gives a string of 1s and 0s in groups of 8.  This is most likely binary with each group of 8 bits (1byte) representing a character:
```console
leviathan4@gibson:~$ ls -la .trash/bin
-r-sr-x--- 1 leviathan5 leviathan4 14940 Apr 10 14:23 .trash/bin
leviathan4@gibson:~$ ./.trash/bin
00110000 01100100 01111001 01111000 01010100 00110111 01000110 00110100 01010001 01000100 00001010
```

We can quite easily convert the binary strings into their equivalent ASCII characters using an online binary-to-text converter.  I chose to use [Cyberchef](https://gchq.github.io/CyberChef/) for this with the following recipe:
```json
[
  { "op": "From Binary",
    "args": ["Space", 8] }
]
```

This gives us the corresponding ASCII string: `0dyxT7F4QD` and we can succesfully confirm that this is the password for leviathan5.
